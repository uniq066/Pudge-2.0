# Pudge-2.0

```cpp
#include <iostream>
#include <string>
#include <cstdlib> // Для rand() и srand()
#include <ctime>   // Для time()
using namespace std;

class Pudge {
private:
    int health;
    int armour;
    string* abilities; // Динамический массив строк для способностей
    int abilityCount;  // Количество способностей

public:
    // Конструктор
    Pudge(int initialHealth, int initialArmour, string* initialAbilities, int count) {
        health = initialHealth;
        armour = initialArmour;
        abilityCount = count;
        abilities = new string[abilityCount]; // Выделяем память под массив способностей
        for (int i = 0; i < abilityCount; i++) {
            abilities[i] = initialAbilities[i];
        }
        srand(static_cast<unsigned int>(time(0))); // Инициализация генератора случайных чисел
    }

    // Деструктор
    ~Pudge() {
        delete[] abilities; // Освобождаем память
    }

    // Метод для увеличения здоровья и брони
    void FreshMeat(int healthIncrease, int armourIncrease) {
        health += healthIncrease;
        armour += armourIncrease;
        cout << "Fresh meat consumed! Health increased by " << healthIncrease 
             << " and Armour increased by " << armourIncrease << "." << endl;
    }

    // Метод для получения урона
    void TakeDamage(int damage) {
        int effectiveDamage = max(0, damage - armour); // Учитываем броню
        health -= effectiveDamage;
        cout << "Pudge took " << effectiveDamage << " damage. Health is now " << health << "." << endl;
    }

    // Метод для отображения текущего состояния
    void DisplayStatus() const {
        cout << "Pudge's current status -> Health: " << health << ", Armour: " << armour << endl;
    }

    // Метод для вывода случайной способности
    void DisplayRandomAbility() const {
        int randomIndex = rand() % abilityCount; // Генерируем случайный индекс
        cout << "Random Ability: " << abilities[randomIndex] << endl;
    }

    // Метод для получения количества урона в зависимости от статистики и способности
    int CalculateDamage(int stat, const string& ability) const {
        int baseDamage = 10; // Базовый урон
        if (ability == "Hook") {
            return baseDamage + stat; // Увеличиваем урон на значение статистики
        } else if (ability == "Rot") {
            return baseDamage + (stat / 2); // Урон от Рота
        }
        // Добавьте другие способности по мере необходимости
        return baseDamage; // Если способность не распознана, возвращаем базовый урон
    }
};

// Пример использования класса
int main() {
    string abilities[] = {"Hook", "Rot", "Dismember"}; // Способности Пуджа
    Pudge pudge(100, 10, abilities, 3);
    
    // Выводим начальное состояние
    pudge.DisplayStatus();
    
    // Потребляем свежие мясо
    pudge.FreshMeat(20, 5);
    
    // Выводим состояние после увеличения
    pudge.DisplayStatus();
    
    // Выводим случайную способность
    pudge.DisplayRandomAbility();
    
    // Pudge получает урон
    pudge.TakeDamage(15);
    
    // Вычисляем урон от способности
    int damage = pudge.CalculateDamage(25, "Hook");
    cout << "Damage dealt by Hook: " << damage << endl;

    // Выводим финальное состояние
    pudge.DisplayStatus();

    return 0;
}
```
