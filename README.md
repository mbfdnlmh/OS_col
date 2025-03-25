# OS_col
Коллоквиум ОС
Харченко Анастасия Андреевна 9 группа вариант 2
# Лабораторная работа №2

## 1. Win API, необходимое для решения Лабораторной работы №2
Для работы с многопоточностью и синхронизацией в Windows могут понадобиться следующие функции WinAPI:
- CreateProcess – создание процесса.
- CreateThread – создание потока.
- ExitThread – завершение потока.
- WaitForSingleObject – ожидание завершения потока.
- EnterCriticalSection / LeaveCriticalSection – работа с критическими секциями.
- CreateSemaphore / ReleaseSemaphore – работа с семафорами.
- InterlockedIncrement / InterlockedDecrement – атомарные операции.

## 2. Что такое процесс в ОС Windows?
Процесс – это экземпляр исполняемой программы с собственным виртуальным адресным пространством, системными ресурсами и одним или несколькими потоками выполнения.

## 3. Что такое Критическая секция?
Критическая секция – механизм синхронизации в многопоточных приложениях, позволяющий ограничивать доступ к общим ресурсам, предотвращая состояние гонки.

## 4. Что такое Семафор?
Семафор – механизм синхронизации, используемый для управления доступом к общему ресурсу с возможностью ограничения числа одновременно работающих потоков.

## 5. Сравнительный анализ стандартов C++  
| Стандарт | Без Boost/Qt | С Boost/Qt |
|----------|------------|-----------|
| C++98    | Строгая типизация, ограниченные возможности STL, ручное управление ресурсами | Boost добавляет умные указатели, многопоточность и расширенные контейнеры |
| C++11+   | Автоматическое управление памятью (RAII), умные указатели, лямбда-выражения, std::thread | Qt даёт кроссплатформенный GUI, сигналы и слоты, удобные контейнеры |

# Общие вопросы

## 1) Определение ООП
Объектно-ориентированное программирование (ООП) – парадигма программирования, основанная на концепциях объектов, инкапсуляции, наследования и полиморфизма.

## 2) Магическое число 7 Миллера в IT
1. 7 слотов памяти в регистровой машине.
2. 7 уровней модели OSI.
3. 7 фаз компиляции в C++.
4. 7 слоев безопасности в архитектуре ПО.
5. 7 классов HTTP-статусов.
6. 7 фаз жизненного цикла разработки ПО (SDLC).
7. 7 ключевых принципов SOLID.

## 3) Энтропия ПО  
Примеры негативных энтропийных мер:
1. Использование строгих стайл-гайдов.
2. Нормализация баз данных.
3. Автоматическое тестирование.
4. Использование CI/CD.
5. Статический анализ кода.

## 4) 5 признаков сложной системы по Бучу
1. Иерархичность – система состоит из подсистем (пример: модульный дизайн ПО).
2. Эмерджентность – поведение системы невозможно предсказать (пример: баги в сложных алгоритмах).
3. Адаптивность – система изменяется под требования (пример: динамическая настройка конфигурации).
4. Гетерогенность – разнородные компоненты (пример: микросервисная архитектура).
5. Эволюционность – постепенное развитие системы (пример: версии API).

## 5) Закон иерархических компенсаций Седова в IT
1. Переход от C к C++ из-за сложности поддержки.
2. Введение виртуализации для оптимизации серверов.
3. Появление Docker для борьбы с проблемами окружений.
4. Микросервисы для масштабирования монолитных приложений.
5. Rust как замена C++ для безопасного управления памятью.

# Задачки

## 1) Генерация чисел Фибоначчи
#include <gtest/gtest.h>
#include <vector>

std::vector<int> fibonacci(int n) {
    if (n <= 0) return {};
    std::vector<int> fib{0, 1};
    for (int i = 2; i < n; ++i) {
        fib.push_back(fib[i - 1] + fib[i - 2]);
    }
    return fib;
}
TEST(FibonacciTest, BasicCases) {
    EXPECT_EQ(fibonacci(1), std::vector<int>({0}));
    EXPECT_EQ(fibonacci(2), std::vector<int>({0, 1}));
    EXPECT_EQ(fibonacci(5), std::vector<int>({0, 1, 1, 2, 3}));
}

TEST(FibonacciTest, EdgeCases) {
    EXPECT_EQ(fibonacci(0), std::vector<int>());
    EXPECT_EQ(fibonacci(-5), std::vector<int>());
}
## 2) Палиндром
```
#include <gtest/gtest.h>
#include <string>

bool is_palindrome(int num) {
    std::string str = std::to_string(num);
    return str == std::string(str.rbegin(), str.rend());
}

TEST(PalindromeTest, PositiveCases) {
    EXPECT_TRUE(is_palindrome(121));
    EXPECT_TRUE(is_palindrome(1221));
    EXPECT_TRUE(is_palindrome(1));
}

TEST(PalindromeTest, NegativeCases) {
    EXPECT_FALSE(is_palindrome(123));
    EXPECT_FALSE(is_palindrome(10));
}
```
## 3)Развернуть связный список используя итеративный подход
```
#include <gtest/gtest.h>

struct Node {
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

Node* reverse_list(Node* head) {
    Node* prev = nullptr;
    Node* current = head;
    while (current) {
        Node* next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }
    return prev;
}

std::vector<int> list_to_vector(Node* head) {
    std::vector<int> result;
    while (head) {
        result.push_back(head->data);
        head = head->next;
    }
    return result;
}

TEST(LinkedListTest, ReverseList) {
    Node* head = new Node(1);
    head->next = new Node(2);
    head->next->next = new Node(3);

    Node* reversed = reverse_list(head);
    EXPECT_EQ(list_to_vector(reversed), std::vector<int>({3, 2, 1}));
}
```
