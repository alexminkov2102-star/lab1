# lab1
Тема: Програмні моделі систем координат
Мета роботи:
Спроєктувати та реалізувати імутабельні програмні моделі для представлення точок у 2D та 3D системах координат;
Реалізувати механізми перетворення між декартовою, полярною та сферичною системами координат з використанням статичних фабричних методів;
Навчитись обчислювати відстані між точками, використовуючи різні математичні підходи.
Провести аналіз продуктивності обчислень для різних представлень даних.
Опис класів
CartesianPoint2D
class CartesianPoint2D:
    def __init__(self, x: float, y: float): #конструктор, приймає 2 параметри: цільночислові координати x та y
        self.__x = x
        self.__y = y

    @property
    def x(self): #параметр класу, метод доступу позв ним
        return self.__x

    @property
    def y(self): #параметр класу, метод доступу позв ним
        return self.__y

    @staticmethod #статичний метод конвертації об'єкта класу точки полярних координат у 2д декартову систему
    def from_polar(polar_point: "PolarPoint") -> "CartesianPoint2D":
        return CartesianPoint2D(polar_point.radius * math.cos(polar_point.angle),
                                polar_point.radius * math.sin(polar_point.angle))

    @staticmethod #статичний метод виміру дистанціїї між двома точками - об'єктами класу CartesianPoint2D
    def distance(first_point: "CartesianPoint2D", second_point: "CartesianPoint2D") -> float:
        return math.sqrt(pow((second_point.x - first_point.x),2)
                         + pow((second_point.y - first_point.y),2))
PolarPoint
class PolarPoint:
    def __init__(self, radius: float, angle: float): # конструктор, приймає 2 параметри: радіус (відстань від початку координат) та кут (в радіанах)
        self.__radius = radius
        self.__angle = angle
@property
def radius(self): # параметр класу, метод доступу до радіуса точки
    return self.__radius

@property
def angle(self): # параметр класу, метод доступу до кута точки
    return self.__angle

@staticmethod # статичний метод конвертації об'єкта класу декартової точки у полярну систему координат
def from_cartesian(cartesian_point: "CartesianPoint2D") -> "PolarPoint":
    return PolarPoint(math.sqrt(cartesian_point.x*cartesian_point.x + cartesian_point.y * cartesian_point.y),
                      math.atan2(cartesian_point.y, cartesian_point.x))

@staticmethod # статичний метод виміру відстані між двома точками в полярних координатах за теоремою косинусів
def distance(first_point: "PolarPoint", second_point: "PolarPoint") -> float:
    return math.sqrt(pow(second_point.radius, 2)
                     + pow(first_point.radius, 2)
                     - 2 * first_point.radius * second_point.radius * math.cos(second_point.angle - first_point.angle))
CartesianPoint3D
class CartesianPoint3D:
    def __init__(self, x: float, y: float, z: float): # конструктор, приймає 3 параметри: координати x, y та z у тривимірному просторі
        self.__x = x
        self.__y = y
        self.__z = z

    @property
    def x(self): # параметр класу, метод доступу до координати x
        return self.__x

    @property
    def y(self): # параметр класу, метод доступу до координати y
        return self.__y

    @property
    def z(self): # параметр класу, метод доступу до координати z
        return self.__z

    @staticmethod # статичний метод конвертації об'єкта класу сферичної точки у 3D декартову систему координат
    def from_spherical_point(spherical_point: "SphericalPoint") -> "CartesianPoint3D":
        return CartesianPoint3D(
            spherical_point.radius * math.sin(spherical_point.polar_angle) * math.cos(spherical_point.azimuth),
            spherical_point.radius * math.sin(spherical_point.polar_angle) * math.sin(spherical_point.azimuth),
            spherical_point.radius * math.cos(spherical_point.polar_angle)
        )

    @staticmethod # статичний метод виміру відстані між двома 3D точками за формулою евклідової відстані у тривимірному просторі
    def distance(first_point, second_point) -> float:
        return math.sqrt(pow((second_point.x - first_point.x), 2)
                         + pow((second_point.y - first_point.y), 2)
                         + pow((second_point.z - first_point.z), 2))
SphericalPoint
class SphericalPoint:
    def __init__(self, radius: float, azimuth: float, polar_angle: float): # конструктор, приймає 3 параметри: радіус (відстань від початку координат), азимут (кут в горизонтальній площині) та полярний кут (кут відносно вертикальної осі)
        self.__radius = radius
        self.__azimuth = azimuth
        self.__polar_angle = polar_angle
@property
def radius(self): # параметр класу, метод доступу до радіуса точки
    return self.__radius

@property
def azimuth(self): # параметр класу, метод доступу до азимута точки
    return self.__azimuth

@property
def polar_angle(self): # параметр класу, метод доступу до полярного кута точки
    return self.__polar_angle

@staticmethod # статичний метод конвертації об'єкта класу 3D декартової точки у сферичну систему координат
def from_cartesian_point(cartesian_point: "CartesianPoint3D") -> "SphericalPoint":
    cache_radius = math.sqrt(cartesian_point.x * cartesian_point.x
                             + cartesian_point.y * cartesian_point.y
                             + cartesian_point.z * cartesian_point.z)
    return SphericalPoint(
        cache_radius,
        math.atan2(cartesian_point.y, cartesian_point.x),
        math.atan2(cartesian_point.z, cache_radius)
    )

@staticmethod # статичний метод виміру прямої (евклідової) відстані між двома точками у сферичних координатах
def direct_distance(first_point: "SphericalPoint", second_point: "SphericalPoint") -> float:
    return(
        math.sqrt(abs(
            pow(first_point.radius, 2) + pow(second_point.radius, 2)
            - 2 * first_point.radius * second_point.radius * math.cos(first_point.polar_angle - second_point.polar_angle)
            + math.cos(first_point.azimuth) * math.cos(second_point.azimuth)
        ))
    )

@staticmethod # статичний метод виміру кругової (дугової) відстані між двома точками на сфері одного радіуса, повертає None якщо радіуси різні
def circular_distance(first_point: "SphericalPoint", second_point: "SphericalPoint") -> float | None:
    if first_point.radius != second_point.radius:
        print("Radius of Both point must be equal! \n")
        return None

    return(
        first_point.radius * math.acos(
        math.sin(first_point.polar_angle) * math.sin(second_point.polar_angle) *
        math.cos(first_point.azimuth - second_point.azimuth) +
        math.cos(first_point.polar_angle) * math.cos(second_point.polar_angle)
    )
    )
Інструкції дл компіляці
Встановити Python 3.12.6
Завантажити у віртуально середовище Numpy: pip install numpy
Запуск скрипта
Перевірка коректності


Рисунок 3.1 - Перевірка правильності конвертації
Як можна побачити, із попровкою на похибку конвертації чисел з плаваючею точкою конвертація вірна



Рисунок 3.2 - Результати обчислень відстаней


Рисунок 3.3 - Результати обчислень відстаней
Результати аналізу продуктивності


Рисунок 4.1 - Результати тестування
Тепер напишемо таблицю із тестами та їх результатами:

Type	2D	3D
Polar	0.107s	-
Cartesian	0.075s	0.101s
Spherical direct	-	0.144s
Spherical circular	-	0.114s
Як можна побачити, найшвидшим виявилася операція обчислення дистанції у 2-вимірній декартовій системі координати, коли як найдовший це хордовий метод обчислення у сферичній системі

Проміжний висновок для 2D просторів:
Як можна побачити, формули майже ідентичні, проте наявність тригонометричної функції збільшила час виконання у ~29%

Проміжний висновок для 3D просторів:
Як можна побачити, тенденція збереглася, найшвидший тест декартової системи, хоч і зі збільшеним часов 25%, а у сферичній системі довше. Якщо помітити то майже ідентична формула до полярної відстані, хордова має 2 додаткові косинуси, що сповільнило обчислення відносно полярних систем на ~26%. Проте формула обчислення сферичної дуги має 2 синуса, 3 косинуса, та 1 акосинус, і вона швидше хордової на ~21%, що дає підстави вважати, що функція квадратного корення, може сповільнити обчислення на більший процент чим тригонометричні функції, які також є коштовними з точки зору обрахункових можливостей

Висновки
Після лабороторно-практичної роботи були закріплені теоретичні навички з дисципліни програмування координатних систем, а саме матеріал про різні координатні системи відчислення. Протягом практичної роботи, завдяки потужностям мови програмування Python, були розроблені та протестовані засоби зберігання, конвертації точок чотирьох різних систем координат. Після тестування продуктивності числових формул обчислення відстаней між дома точками, було виявлено - декартова система координат найкраще справляється із цими задачами, як у двовимірному, так і у тривимірному просторі.
