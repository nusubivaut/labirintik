# Отчет по реферату по курсу
# "Фундаментальная информатика"
***Ход работы***

С одной стороны, игры — дело несерьезное. Конференции и конкурсы программных проектов очень редко рассматривают такие работы.
С другой, разработка игр — это сложная задача, включающая в себя много процессов и требующая использовать различные технологии.
Можно утверждать, что любая игра — это Объекты + Графика + Физика + ...
Для того чтобы было удобно программировать все это многообразие, используют фреймворки. Одним из самых известных и популярных фреймворков для разработки игр на Python является Pygame.

Разработка pygame началась в 2000 году Питером Шиннерсом. Тогда он активно программировал на Си и познакомился с Python (версии 1.5.2) и библиотекой SDL. В то время она была очень популярна, на ней разрабатывались сотни игр, в том числе коммерческих, и идея подключить ее к Python показалась ему очень интересной.
Начиная примерно с 2004-2005 года, pygame поддерживается и развивается сообществом свободного программного обеспечения.
Pygame включает в себя все, что необходимо для разработки игр: удобную работу с графикой (например, поддержку спрайтов), c методами-детекторами столкновений, звук и многое другое.

Для начала работы нужно подключить модуль pygame, вызвать функцию init()и создать холст.
Например:
import pygame

```
if __name__ == '__main__':
    pygame.init()
    size = width, height = 800, 600
    screen = pygame.display.set_mode(size)
```
И теперь можно свободно рисовать, вернее, формировать кадр на холсте с именем screen.
Что же всегда волновало авторов компьютерных игр? Правильно, их очень заботила проблема сделать движения героев, смены кадров и прочие анимационные вещи плавными, то есть создать эффект анимации.
Для того чтобы движение объектов было плавным, pygame использует двойную буферизацию. Пока пользователь смотрит на один кадр, следующий уже строится в памяти, при этом у пользователя ничего не меняется. Как только новый кадр нарисован, он помещается на видимый экран. Это происходит очень быстро, и пользователь не видит процесса рисования, который на большой скорости обновления выглядит как мерцание.
Для пользователя смена изображений происходит мгновенно.
Функция ```flip()``` как раз и выполняет эту смену. Поэтому после отрисовки кадра обязательно требуется команда:
```pygame.display.flip()```
Давайте дождёмся того момента, когда пользователь закроет окно. Сделаем это так:
```
while pygame.event.wait().type != pygame.QUIT:
    pass
```
Конечно, при оценке игры в первую очередь смотрят на ее графику. Обычно в играх используют готовые изображения, которые загружаются из дополнительных файлов.     
    
Давайте вспомним, как устроены цвета в компьютерном мире.
Рисование на экране в конечном итоге — это появление пикселей (точек) разного цвета. Практически все команды рисования принимают цвет в качестве параметра.
Цвета каждого пикселя на мониторах формируется тремя световыми источниками:
⦁	Красным
⦁	Синим
⦁	Зеленым
Каждая компонента-источник может иметь 256 градаций интенсивности (0 — совсем не горит, 255 — горит очень ярко).
Например,
(0, 0, 0) — это черный цвет,
(255, 255, 255) — белый цвет.
Вообще, все цвета, имеющие одинаковые значения компонент — серые разной интенсивности.
(255, 0, 0) — красный,
(255, 100, 100) — светло-красный.
Для цветов в pygame используется отдельный модуль Color.
Цвета в pygame — это объекты типа Color. Их могут создавать конструкторы:
Color(name)       # строкой, например, "yellow"
Color(r, g, b, a) # красный, зеленый, синий и прозрачность
Color(rgbvalue)
Последний конструктор может принимать как число, соответствующее цвету, так и строки в HTML-формате (#rrggbbaa или #rrggbb).
Таким образом, для создания цветов можно использовать такой код:
```
lightred = pygame.Color(255, 255, 100)
darkgreen = pygame.Color('#008000')
yellow = pygame.Color('yellow')
```
В Интернете есть много инструментов по RGB-представлению цветов.
Полезно знать, что существуют и другие форматы представления цвета (не только RGB).
Очень важным является HSV формат (```Hue, Saturation, Value``` — тон, насыщенность, значение) или HSB (```Hue, Saturation, Brightness``` — тон, насыщенность, яркость).
Возможные значения в диапазонах: H = [0, 360], S = [0, 100], V = [0, 100].

Экран pygame устроен обычно для компьютерных графических систем (и необычно для математиков). Точка (0;0) — в верхнем левом углу, а ось Y направлена вниз.

В pygame рисование происходит на Surface. Хоть это слово дословно переводится как Поверхность, мы чаще будем использовать термин Холст. Объект именно этого класса возвращается методом ```set_mode()``` модуля ```display```.

Мы будем достаточно редко вызывать методы этого объекта, чаще передавать его в функции рисования модуля ```draw```, но один метод используется чаще других.
При помощи метода ```fill()``` можно залить весь холст. Ему передается цвет заливки. 

Обычно программа на ```Pygame```, даже если она показывает статичную картинку, все равно содержит игровой цикл.
Главный игровой цикл — обязательный компонент любой игры. В нем происходит постоянная отрисовка игровых объектов, изменение их состояния (например, положения) и обработка событий. Прежде всего, цикл реагирует на действия пользователя.
Общая концепция примерно такая же, как при использовании ```PyQT```: мы задаем реакцию приложения на определенные события, только там главный цикл запускался неявно через app.exec(), тут же нам предстоит более низкоуровневое управление этой частью программы.

Не имеет большого значения, с какой скоростью мерцает телевизор из предыдущего примера. Но в играх время играет очень важную роль. На некоторых машинах движение будет идти слишком быстро, на других — слишком медленно. Это зависит как от мощности компьютера, так и от загруженности процессора.
Но разработчик игры стремится к тому, чтобы на любом компьютере движение выглядело примерно одинаково. Для этого нужно учитывать время.

В Pygame для учета времени есть специальный класс ```Clock``` в модуле ```time```.
Нужно создать его экземпляр перед игровым циклом, а в самом цикле на каждом шаге вызывать метод ```tick()``` этого экземпляра.
Этот метод возвращает количество миллисекунд, прошедших с момента последнего вызова. Можно ориентироваться на него и работать с объектом игры с учетом полученного прошедшего времени.


В ```Pygame``` есть модули ```mouse``` и ```keyboard```. Они позволяют «опрашивать» мышь и клавиатуру в любой момент, то есть получать от устройств информацию. Но удобнее работать с событиями.
Важнее узнать, что кнопка мыши нажалась, чем получить информацию о том, что она нажата.
Любая игра также управляется событиями. Что же это за события?
Прежде всего, это события пользовательского ввода: игрок нажал клавишу на клавиатуре, подвинул мышь, нажал на кнопку закрытия окна и т. д. На каждом шаге главного игрового цикла мы разбираем накопившиеся события.
Несмотря на то, что цикл работает очень быстро, за одну итерацию наступивших событий может быть несколько. Поэтому в программе появляется второй внутренний цикл, который обрабатывает все произошедшие события (разбирает очередь событий).

Иногда требуется создавать свои собственные события, которые должны возникать с определенной периодичностью. Например, каждые 10 миллисекунд необходимо проверять значение некоторой переменной, которую могут менять различные обработчики.
Для этого есть следующий механизм:
⦁	Объявляем свое событие. Это целочисленная константа, ее значение должно находиться между значениями констант pygame.USEREVENT и pygame.NUMEVENTS
⦁```	MYEVENTTYPE = pygame.USEREVENT + 1```
⦁	Вызываем функцию
⦁	```pygame.time.set_timer(MYEVENTTYPE, 10)```
⦁	Обрабатываем событие в основном цикле игры так же, как и другие стандартные события
⦁	```
for event in pygame.event.get():
	    if event.type == MYEVENTTYPE:
	        print("Мое событие сработало")```
⦁	Если в какой-то момент необходимо отменить генерацию этого события, необходимо вызвать эту же функцию и передать ей в качестве аргумента 0
⦁	```pygame.time.set_timer(MYEVENTTYPE, 0)```

в ```Pygame``` нет специального класса ```Image``` (как, например, в PIL или в PyQT), а изображения представлены объектами класса ``````Surface```, с которым мы познакомились еще на самом первом занятии. Чтобы создать простую картинку и вывести ее на экран, можно поступить так:
# загруженные изображения — это, фактически, обычный Surface
```
image = pygame.Surface([100, 100])
image.fill(pygame.Color("red"))
...
screen.blit(image, (10, 10))
```
Но, конечно, для нас интерес представляет именно загрузка готовых изображений. Это делается с помощью функции load() модуля image. Давайте напишем к ней собственную функцию-обертку, которая будет загружать изображение по имени файла, в котором оно хранится.
Пусть ее сигнатура будет следующей:
```def load_image(name, colorkey=None)```
Помимо имени изображения у функции будет еще один необязательный параметр — colorkey, который мы будем использовать чуть позже, чтобы обрезать фон изображения.
Игровые ресурсы (картинки, звуковые файлы, видеофрагменты) принято хранить в специальной папке отдельно от кода программы.
Условимся, что мы храним все ресурсы в папке data.
Кроме того, нужно учесть, что если файла с указанным именем не существует, нужно вывести сообщение об ошибке.

Обычно картинки должны быть с прозрачным фоном. Чтобы человечек выглядел именно человечком, а не черным или белым квадратиком, на котором нарисован человечек.
Если при этом изображение уже прозрачно (это обычно бывает у картинок форматов png и gif), то после загрузки вызываем функцию ```convert_alpha()```, и загруженное изображение сохранит прозрачность.
Если изображение было непрозрачным, то используем функцию ```Surfaceset_colorkey(colorkey)```, и тогда переданный ей цвет станет прозрачным.

Но во многих играх объекты имеют произвольные размеры (не привязаны к клеткам), и при этом важно управлять движением и взаимоотношениями нескольких объектов (например, столкновениями: выстрел попадает в танк, человечек подходит к лестнице и т.д.). В этом нам помогут спрайты.
Назовем спрайтом произвольный игровой графический объект. Этот объект может перемещаться по игровому полю или быть неподвижным, им может управлять игрок, или же его поведение контролируется непосредственно алгоритмом игры.
У спрайтов нет функции ```draw()```. Для того чтобы работать со спрайтами, их объединяют в группы. Потом достаточно отрисовать группу, и все спрайты, принадлежащие группе, будут отрисованы (мы уже сталкивались с подобным принципом). Спрайт может и обычно принадлежит нескольким группам одновременно.
При создании спрайта нужно не забыть задать его вид ```(image)``` и размер ```(rect)```. Обычно для определения размеров берут прямоугольник, ограничивающий загруженное изображение.
Для работы со спрайтами в Pygame есть специальный модуль — sprite. По ссылке есть много интересной информации и дополнительных примеров.
В простейшем случае спрайт можно создать так:
# создадим группу, содержащую все спрайты
```all_sprites = pygame.sprite.Group()```

# создадим спрайт
```sprite = pygame.sprite.Sprite()```
# определим его вид
```sprite.image = load_image("bomb.png")```
# и размеры
```sprite.rect = sprite.image.get_rect()```
# добавим спрайт в группу
```all_sprites.add(sprite)```
Для размещения спрайта на холсте надо задать координаты его левого верхнего угла:
```sprite.rect.x = 5```
```sprite.rect.y = 20```
При этом размеры спрайта нигде не указываются, а определяются размерами картинки, которая спрайт создает.
Как уже говорилось, для изменения размеров самой картинки можно применять функцию ```scale()``` модуля ```transform```, но такой способ не рекомендуется.

Работа со спрайтами станет значительно проще, если использовать «объектный» подход и унаследовать новый класс-спрайт от ```pygame.sprite.Sprite```.
При наследовании важно не забыть вызвать конструктор базового класса. Иначе мы получим ошибку:
```AttributeError: 'mysprite' instance has no attribute '_Sprite__g'```
В производном классе можно добавить свои функции, а также переопределить функцию update(). Тогда при вызове этой функции для группы, произойдет вызов update() для каждого спрайта, который входит в нее.


Когда речь заходит об играх, то обязательно всплывает тема о физических процессах, которые лежат в основе поведения игровых объектов. Движение объектов в поле других объектов, столкновения объектов и другая функциональность — все это требует физического и математического моделирования.
Например, определение столкновений — это определение пересечений геометрических фигур.
В Pygame не реализована поддержка физических процессов, но в модуле spriteсуществуют удобные функции для определения столкновений объектов.

Какие же возможности дает библиотека Pygame программисту?
Во-первых, Pygame позволяет проверить спрайт на столкновение с другим спрайтом. Причем проверить это можно двумя способами:
⦁	По ограничивающему прямоугольнику (метод ```collide_rect()```)
⦁	По ограничивающей окружности (метод ```collide_circle()```)
Для «рядовых ситуаций» этого достаточно.
А во-вторых, Pygame может проверить спрайт на столкновение с группой других спрайтов.

Не все объекты имеют простую форму, поэтому их обработку не всегда можно свести к проверке столкновений ограничивающих объекты прямоугольников и окружностей.
Для таких сложных объектов возможно использовать функцию ```pygame.sprite.collide_mask()```, которая ориентируется именно на пересечение картинок. Чтобы ее использовать, стоит заранее вычислить маску в проверяемых на столкновение спрайтах.
```sprite.mask = pygame.mask.from_surface(sprite.image)```
Или, если в конструкторе:
```self.mask = pygame.mask.from_surface(self.image)```

Практически каждая игра состоит из нескольких экранов, на которых происходит действие. Такими экранами являются заставка и конец игры.
На отдельном экране (а в небольших играх — прямо на экране) заставки можно расположить правила. Небольшая проблема в том, что Pygame не умеет выводить несколько строк одновременно, приходится делать это построчно вручную.
В функции start_screen(), приведенной ниже, мы организовали свой мини-игровой цикл, который выполняется до тех пор, пока игрок не нажмет клавишу на клавиатуре или кнопку мыши.
На экране окончания игры обычно выводится итоговый счет, авторы игры, прочая рекламная информация.
Преждевременный выход из игры возможен на любом экране, поэтому удобно «аварийное завершение» оформить в виде отдельной функции ```terminate()```:
```
FPS = 50

def terminate():
    pygame.quit()
    sys.exit()

def start_screen():
    intro_text = ["ЗАСТАВКА", "",
                  "Правила игры",
                  "Если в правилах несколько строк,",
                  "приходится выводить их построчно"]

    fon = pygame.transform.scale(load_image('fon.jpg'), (WIDTH, HEIGHT))
    screen.blit(fon, (0, 0))
    font = pygame.font.Font(None, 30)
    text_coord = 50
    for line in intro_text:
        string_rendered = font.render(line, 1, pygame.Color('white'))
        intro_rect = string_rendered.get_rect()
        text_coord += 10
        intro_rect.top = text_coord
        intro_rect.x = 10
        text_coord += intro_rect.height
        screen.blit(string_rendered, intro_rect)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                terminate()
            elif event.type == pygame.KEYDOWN or \
                    event.type == pygame.MOUSEBUTTONDOWN:
                return  # начинаем игру
        pygame.display.flip()
        clock.tick(FPS)
```

# Код
```
import pygame
import sys
from random import choice, randrange

RES = shirot, visot = 1202, 800
for_del = 100
_c_o_l_s, rows = shirot // for_del, visot // for_del
gameeeeeeeeee = 0


class Cell:
    def __init__(self, x, y):
        self.x, self.y = x, y
        self.walls = {'top': True, 'right': True, 'bottom': True, 'left': True}
        self.visited = False
        self.thickness = 4

    def draw(self, sc):
        x, y = self.x * for_del, self.y * for_del

        if self.walls['top']:
            pygame.draw.line(sc, pygame.Color('yellow'), (x, y), (x + for_del, y), self.thickness)
        if self.walls['right']:
            pygame.draw.line(sc, pygame.Color('yellow'), (x + for_del, y), (x + for_del, y + for_del), self.thickness)
        if self.walls['bottom']:
            pygame.draw.line(sc, pygame.Color('yellow'), (x + for_del, y + for_del), (x, y + for_del), self.thickness)
        if self.walls['left']:
            pygame.draw.line(sc, pygame.Color('yellow'), (x, y + for_del), (x, y), self.thickness)

    def get_rects(self):
        acid_rain = []
        x, y = self.x * for_del, self.y * for_del
        if self.walls['top']:
            acid_rain.append(pygame.Rect((x, y), (for_del, self.thickness)))
        if self.walls['right']:
            acid_rain.append(pygame.Rect((x + for_del, y), (self.thickness, for_del)))
        if self.walls['bottom']:
            acid_rain.append(pygame.Rect((x, y + for_del), (for_del, self.thickness)))
        if self.walls['left']:
            acid_rain.append(pygame.Rect((x, y), (self.thickness, for_del)))
        return acid_rain

    def check_cell(self, x, y):
        find_index = lambda x, y: x + y * _c_o_l_s
        if x < 0 or x > _c_o_l_s - 1 or y < 0 or y > rows - 1:
            return False
        return self.grid_cells[find_index(x, y)]

    def check_neighbors(self, grid_cells):
        self.grid_cells = grid_cells
        neighbors = []
        top = self.check_cell(self.x, self.y - 1)
        right = self.check_cell(self.x + 1, self.y)
        bottom = self.check_cell(self.x, self.y + 1)
        left = self.check_cell(self.x - 1, self.y)
        if top and not top.visited:
            neighbors.append(top)
        if right and not right.visited:
            neighbors.append(right)
        if bottom and not bottom.visited:
            neighbors.append(bottom)
        if left and not left.visited:
            neighbors.append(left)
        return choice(neighbors) if neighbors else False


def remove_walls(current, next):
    dx = current.x - next.x
    if dx == 1:
        current.walls['left'] = False
        next.walls['right'] = False
    elif dx == -1:
        current.walls['right'] = False
        next.walls['left'] = False
    dy = current.y - next.y
    if dy == 1:
        current.walls['top'] = False
        next.walls['bottom'] = False
    elif dy == -1:
        current.walls['bottom'] = False
        next.walls['top'] = False


def generate_maze():
    grid_cells = [Cell(col, row) for row in range(rows) for col in range(_c_o_l_s)]
    current_cell = grid_cells[0]
    array = []
    break_count = 1

    while break_count != len(grid_cells):
        current_cell.visited = True
        next_cell = current_cell.check_neighbors(grid_cells)
        if next_cell:
            next_cell.visited = True
            break_count += 1
            array.append(current_cell)
            remove_walls(current_cell, next_cell)
            current_cell = next_cell
        elif array:
            current_cell = array.pop()
    return grid_cells


spisok_zlodeev = ['darkar.png', 'cherni.png', 'diaspora.png', 'triton.png', 'trix1.png', 'valtor.png']
random_zlodei = choice(spisok_zlodeev)


class Food:
    def __init__(self):
        self.img = pygame.image.load('valtor.png').convert_alpha()
        self.img = pygame.transform.scale(self.img, (for_del - 10, for_del - 10))
        self.rect = self.img.get_rect()
        self.set_pos()

    def set_pos(self):
        self.rect.topleft = randrange(_c_o_l_s) * for_del + 5, randrange(rows) * for_del + 5

    def draw(self):
        game_surface.blit(self.img, self.rect)


def is_collide(x, y):
    tmp_rect = player_rect.move(x, y)
    if tmp_rect.collidelist(walls_collide_list) == -1:
        return False
    return True


def eat_food():
    for food in food_list:
        if player_rect.collidepoint(food.rect.center):
            #if_ono_tebya = pygame.mixer.Sound('death.wav')

            #if_ono_tebya.set_volume(0.5)

            #if_ono_tebya.play()
            food.set_pos()
            return True
    return False


def is_game_over():
    global time, score, record, FPS
    if time < 0:
        pygame.time.wait(700)
        player_rect.center = for_del // 2, for_del // 2
        [food.set_pos() for food in food_list]
        set_record(record, score)
        record = get_record()
        time, score, FPS = 120, 0, 60


def get_record():
    try:
        with open('record') as f:
            return f.readline()
    except FileNotFoundError:
        with open('record', 'w') as f:
            f.write('0')
            return 0


def set_record(record, score):
    rec = max(int(record), score)
    with open('record', 'w') as f:
        f.write(str(rec))


class Background(pygame.sprite.Sprite):
    def __init__(self, image_file, location):
        pygame.sprite.Sprite.__init__(self)  # call Sprite initializer
        self.image = pygame.image.load(image_file)
        self.rect = self.image.get_rect()
        self.rect.left, self.rect.top = location


def start_screen():
    global gameeeeeeeeee
    vstuplenie = ["                                        лабиринт только для феечек! ", " ",
                  "",
                  "",
                  ]
    nevstuplenie = [" ", "  ",
                    " "]
    # screen = pygame.display.set_mode([WIN_WIDTH, WIN_HEIGHT])
    datvidania = Background('general_fone.png', [0, 0])
    # fon = pygame.transform.scale(image.load('images/fon.jpg'), (DISPLAY))
    surface.blit(datvidania.image, datvidania.rect)
    font = pygame.font.Font(None, 40)
    text_c = 80
    if gameeeeeeeeee == 0:
        for line in vstuplenie:
            string_rendered = font.render(line, 1, (250, 250, 250))
            o_rect = string_rendered.get_rect()
            text_c += 30
            o_rect.top = text_c
            o_rect.x = 160
            text_c += o_rect.height
            surface.blit(string_rendered, o_rect)
        for e in pygame.event.get():
            if e.type == pygame.QUIT:
                pygame.quit()
                # raise (SystemExit, "QUIT")
                sys.exit()
            if e.type == pygame.MOUSEBUTTONDOWN:
                gameeeeeeeeee = 1

        pygame.display.flip()
    elif gameeeeeeeeee == 2:
        for line in nevstuplenie:
            string_rendered = font.render(line, 1, (0, 0, 240))
            intro_rect = string_rendered.get_rect()
            text_c += 30
            intro_rect.top = text_c
            intro_rect.x = 160
            text_c += intro_rect.height
            surface2.blit(string_rendered, intro_rect)
        for e in pygame.event.get():
            if e.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            pygame.display.flip()


FPS = 60
pygame.init()
game_surface = pygame.Surface(RES)
surface = pygame.display.set_mode((shirot + 300, visot))
surface2 = pygame.display.set_mode((shirot + 300, visot))
clock = pygame.time.Clock()

# images
fone_it = pygame.image.load('general_fone.png').convert()
starssssss = pygame.image.load('stars.png').convert()

# get maze
maze = generate_maze()
feechki = ['blum.png', 'roxi.png', 'muza.png', 'stella.png', 'techna.png', 'leila.png', 'daphna.png', 'flora.png']
spisok_feechek = choice(feechki)
# player settings
player_speed = 6
player_img = pygame.image.load('blum.png').convert_alpha()
player_img = pygame.transform.scale(player_img, (for_del - 2 * maze[0].thickness, for_del - 2 * maze[0].thickness))
player_rect = player_img.get_rect()
player_rect.center = for_del // 2, for_del // 2
directions = {'a': (-player_speed, 0), 'd': (player_speed, 0), 'w': (0, -player_speed), 's': (0, player_speed)}
keys = {'a': pygame.K_a, 'd': pygame.K_d, 'w': pygame.K_w, 's': pygame.K_s}
direction = (0, 0)

# food settings
food_list = [Food() for i in range(10)]

# collision list
walls_collide_list = sum([cell.get_rects() for cell in maze], [])

# timer, score, record
pygame.time.set_timer(pygame.USEREVENT, 1000)
time = 120
score = 0
record = get_record()

# fonts
font = pygame.font.SysFont('Impact', 150)
text_font = pygame.font.SysFont('Impact', 80)

#spisok_pesen = ['да, а что вы хотели.wav', 'Billie Eilish-Therefore I Am-kissvk.com.wav']
#spisok_for_random = choice(spisok_pesen)
#pesenki = pygame.mixer.Sound(spisok_for_random)
#pesenki.set_volume(0.1)

#pesenki.play()
while True:

    [cell.draw(game_surface) for cell in maze]
    if gameeeeeeeeee == 0:
        start_screen()
    elif gameeeeeeeeee == 1:
        surface.blit(starssssss, (shirot, 0))
        surface.blit(game_surface, (0, 0))
        game_surface.blit(fone_it, (0, 0))

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                exit()
            if event.type == pygame.USEREVENT:
                time -= 1

        # controls and movement
        pressed_key = pygame.key.get_pressed()
        for key, key_value in keys.items():
            if pressed_key[key_value] and not is_collide(*directions[key]):
                direction = directions[key]
                break
        if not is_collide(*direction):
            player_rect.move_ip(direction)

        # gameplay
        if eat_food():
            FPS += 10
            score += 1
        is_game_over()

        # draw player
        game_surface.blit(player_img, player_rect)

        # draw food
        [food.draw() for food in food_list]

        # draw stats
        surface.blit(text_font.render('TIME', True, pygame.Color('cyan'), True), (shirot + 70, 20))
        surface.blit(font.render(f'{time}', True, pygame.Color('cyan')), (shirot + 70, 100))
        surface.blit(text_font.render('score:', True, pygame.Color('forestgreen'), True), (shirot + 50, 290))
        surface.blit(font.render(f'{score}', True, pygame.Color('forestgreen')), (shirot + 70, 370))
        surface.blit(text_font.render('record:', True, pygame.Color('magenta'), True), (shirot + 30, 550))
        surface.blit(font.render(f'{record}', True, pygame.Color('magenta')), (shirot + 70, 630))

        # print(clock.get_fps())
        pygame.display.flip()
        clock.tick(FPS)
 ```





