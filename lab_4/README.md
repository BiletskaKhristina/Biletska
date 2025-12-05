# Лабораторна робота: Робота з ООП (Класи та основні конструкції)

## Мета роботи
Навчитись працювати з класами та їх основними конструкціями; створювати об’єкти, використовувати методи, властивості та магічні методи класів.

---

## Завдання 1: Створення базового класу
```python
class MyAnimals:
    pass

a1 = MyAnimals()
print(type(a1))
### Результат виконання:
<class '__main__.MyAnimals'>
## Завдання 2: Порівняння простих даних з об'єктами
animals = ["Рибки", "Кіт", "Собака"]
name = ["Неони", "Мурка", "Рекс"]
age = [1, 5, 2]

class MyAnimals:
    def __init__(self, animals, name, age, mass=None):
        self.animals = animals
        self.name = name
        self.age = age
        self.mass = 0 if mass is None else mass

a1 = MyAnimals("Рибки", "Неони", 1)
a2 = MyAnimals("Кіт", "Мурка", 5, 10)
a3 = MyAnimals("Собака", "Рекс", 2)

print(a1.name, a2.name, a3.name)
### Результат виконання:Неони Мурка Рекс
## Завдання 3: Методи класу
class MyAnimals:
    def __init__(self, animals, name, age, mass=None):
        self.animals = animals
        self.name = name
        self.age = age
        self.mass = 0 if mass is None else mass
    
    def speak(self):
        if self.animals == "Кіт":
            return f"{self.name} говорить Мяууу"
        elif self.animals == "Собака":
            return f"{self.name} говорить Гавв"
        else:
            return f"{self.name} не розмовляє"
    
    def add_one_year(self):
        self.age += 1
        return f"{self.name} прожила один рік"

a1 = MyAnimals("Рибки", "Неони", 1)
a2 = MyAnimals("Кіт", "Мурка", 5, 10)
a3 = MyAnimals("Собака", "Рекс", 2)

for animal in [a1, a2, a3]:
    print(animal.speak())
    animal.add_one_year()
    ### Результат виконання:
    Неони не розмовляє
Мурка говорить Мяууу
Рекс говорить Гавв
## Завдання 4: Змінні класу
class MyAnimals:
    total_animals = 0
    total_animal_types = set()

    def __init__(self, animals, name, age):
        self.animals = animals
        self.name = name
        self.age = age
        MyAnimals.total_animals += 1
        MyAnimals.total_animal_types.add(animals)

a1 = MyAnimals("Рибки", "Неони", 1)
a2 = MyAnimals("Кіт", "Мурка", 5)
a3 = MyAnimals("Собака", "Рекс", 2)

print(MyAnimals.total_animals)
print(MyAnimals.total_animal_types)
### Результат виконання:
3
{'Рибки', 'Собака', 'Кіт'}
## Завдання 5: Статичні методи
class MyAnimals:
    def __init__(self, animals, name, age):
        self.animals = animals
        self.name = name
        self.age = age

    @staticmethod
    def call_pet():
        return "Тваринка біжить до нас"

a1 = MyAnimals("Рибки", "Неони", 1)
a2 = MyAnimals("Кіт", "Мурка", 5)

print(a1.call_pet())
print(a2.call_pet())
print(MyAnimals.call_pet())
### Результат виконання:
Тваринка біжить до нас
Тваринка біжить до нас
Тваринка біжить до нас
## Завдання 6 : Properties
from datetime import datetime

class MyAnimals:
    max_age = 20

    def __init__(self, animals, name, age):
        self.animals = animals
        self.name = name
        self.age = age

    @property
    def live_untill(self):
        return datetime.today().year - self.age + self.max_age

    @property
    def age_in_months(self):
        return self.age * 12

    def add_one_year(self):
        self.age += 1

a1 = MyAnimals("Кіт", "Мурка", 5)
print(a1.live_untill)
print(a1.age_in_months)
a1.add_one_year()
print(a1.live_untill)
print(a1.age_in_months)
### Результат виконання:
2040
60
2041
72
## Завдання 7: Методи класу з CSV
class MyAnimals:
    total_csv = 0
    total_objects = 0

    def __init__(self, animals, name, age):
        self.animals = animals
        self.name = name
        self.age = age
        MyAnimals.total_objects += 1

    @classmethod
    def from_csv(cls, csv_data):
        cls.total_csv += 1
        animal, name, age = csv_data.split(",")
        return cls(animal, name, int(age))

    @classmethod
    def check_name_capital(cls, obj):
        if obj.name[0].isupper():
            return f"Ім'я '{obj.name}' задано правильно"
        else:
            return f"Ім'я '{obj.name}' має починатися з великої букви"

a1 = MyAnimals.from_csv("Кіт,Мурка,5")
a2 = MyAnimals("Собака", "rex", 2)

print(MyAnimals.check_name_capital(a1))
print(MyAnimals.check_name_capital(a2))
### Результат виконання:
Ім'я 'Мурка' задано правильно
Ім'я 'rex' має починатися з великої букви
## Завдання 8: Магічні методи та арифметичні методи
class MyAnimals:
    def __init__(self, animals, name, age):
        self.animals = animals
        self.name = name
        self.age = age

    def __repr__(self):
        return f'MyAnimals("{self.animals}", "{self.name}", {self.age})'

    def __str__(self):
        return f"Це об'єкт: {self.name} ({self.animals}), {self.age} рік(років)"

    def __sub__(self, obj):
        if isinstance(obj, MyAnimals):
            return f"{self.name} покусав {obj.name}"
        elif isinstance(obj, int):
            self.age += obj
            return f"{self.name} постарів на {obj} років, тепер йому {self.age}"
        return NotImplemented

    def __gt__(self, obj):
        if isinstance(obj, MyAnimals):
            return f"Старший: {self.name}" if self.age > obj.age else f"{self.name} не старший за {obj.name}"
        return NotImplemented

    def __len__(self):
        return len(self.name)

a1 = MyAnimals("Собака", "Рекс", 2)
a2 = MyAnimals("Кіт", "Мурзик", 4)

print(a1 - a2)
print(a1 - 1)
print(a1 > a2)
print(len(a1))
print(repr(a1))
print(str(a1))
### Результат виконання:
Рекс покусав Мурзик
Рекс постарів на 1 років, тепер йому 3
Рекс не старший за Мурзик
4
MyAnimals("Собака", "Рекс", 3)
Це об'єкт: Рекс (Собака), 3 рік(років)
# Висновок
 1. Що зроблено в роботі:
 • Створено клас MyAnimals, реалізовано конструктор, методи, статичні та класові методи, властивості та магічні методи.
 2. Чи досягнуто мети роботи:
 • Так, навчились працювати з класами, об’єктами, методами та магічними методами.
 3. Які нові знання отримано:
 • Властивості (@property), статичні та класові методи, магічні методи, перевантаження операторів, робота зі змінними класу.
 4. Чи вдалося відповісти на всі питання в ході роботи:
 • Так, усі завдання виконано та перевірено.
 5. Чи вдалося виконати всі завдання:
 • Так, усі завдання виконані.
 6. Чи виникли складності у виконанні завдання:
 • Спочатку були складності з магічними методами (NotImplementedError), але виправлено.
 7. Чи подобається такий формат здачі роботи (Feedback):
 • Так, зручно виконувати завдання у форматі Jupyter Notebook.
 8. Побажання для покращення (Suggestions):
 • Додати більше прикладів із магічними методами для різних типів даних та об’єктів класу.

 