# Dictionary<TKey, TValue>
Упрощенный класс Dictionary из C# с комментариями.

```cs
public class SimpleDictionary<TKey, TValue>
{
    // Структура Entry хранит информацию для каждого элемента словаря
    private struct Entry
    {
        public int hashCode;  // Хеш-код ключа, используется для определения позиции в массиве _buckets
        public int next;      // Индекс следующего элемента в списке. Используется при коллизиях
        public TKey key;      // Ключ элемента словаря
        public TValue value;  // Значение элемента словаря
    }

    private int[] _buckets;  // Массив бакетов (ведер). Хранит индексы элементов массива _entries
    private Entry[] _entries;  // Массив элементов. Содержит все данные словаря
    private int _count;        // Текущее количество элементов в словаре
    private int _freeList;     // Индекс первого свободного элемента в _entries

    public SimpleDictionary(int capacity)
    {
        _buckets = new int[capacity];  // Инициализация массива бакетов
        _entries = new Entry[capacity];  // Инициализация массива элементов
        _freeList = -1;  // Начальное значение для списка свободных элементов
    }

    public void Add(TKey key, TValue value)
    {
        int hashCode = key.GetHashCode() & 0x7FFFFFFF;  // Получаем неотрицательный хеш-код ключа
        int targetBucket = hashCode % _buckets.Length;  // Определяем индекс бакета

        int index;
        if (_freeList != -1)  // Проверяем, есть ли свободные места в _entries
        {
            index = _freeList;  // Используем свободное место
            _freeList = _entries[index].next;  // Обновляем _freeList
        }
        else
        {
            if (_count == _entries.Length)  // Если массив _entries заполнен, увеличиваем его размер
            {
                Resize();
                targetBucket = hashCode % _buckets.Length;
            }
            index = _count++;  // Используем следующий свободный индекс
        }

        // Добавляем элемент в словарь
        _entries[index].hashCode = hashCode;
        _entries[index].next = _buckets[targetBucket] - 1;
        _entries[index].key = key;
        _entries[index].value = value;
        _buckets[targetBucket] = index + 1;  // Обновляем индекс в бакете
    }

    // Индексатор для доступа к значениям по ключам
    public TValue this[TKey key]
    {
        get
        {
            int index = FindEntry(key);
            if (index >= 0)
            {
                return _entries[index].value;
            }
            throw new InvalidOperationException("Ключ не найден.");
        }
    }

    // Метод для поиска индекса элемента по ключу
    private int FindEntry(TKey key)
    {
        int hashCode = key.GetHashCode() & 0x7FFFFFFF;
        int bucket = hashCode % _buckets.Length;  // Определяем бакет
        int index = _buckets[bucket] - 1;

        // Итерируемся по элементам в цепочке бакета
        while (index >= 0)
        {
            // Проверяем на соответствие хеш-кода и равенство ключей
            if (_entries[index].hashCode == hashCode && _entries[index].key.Equals(key))
            {
                return index;  // Возвращаем индекс найденного элемента
            }
            index = _entries[index].next;  // Переходим к следующему элементу в цепочке
        }
        return -1;  // Возвращаем -1, если элемент не найден
    }

    // Метод для расширения массивов _buckets и _entries
    private void Resize()
    {
        int newSize = _count * 2;  // Удваиваем размер
        var newBuckets = new int[newSize];
        var newEntries = new Entry[newSize];

        Array.Copy(_entries, 0, newEntries, 0, _count);  // Копируем старые данные в новый массив

        for (int i = 0; i < _count; i++)
        {
            if (newEntries[i].hashCode >= 0)
            {
                int bucket = newEntries[i].hashCode % newSize;
                newEntries[i].next = newBuckets[bucket] - 1;
                newBuckets[bucket] = i + 1;
            }
        }

        _buckets = newBuckets;  // Заменяем старые массивы новыми
        _entries = newEntries;
    }

    // Другие методы, такие как Remove, TryGetValue и т.д., могут быть добавлены по мере необходимости
}
```
