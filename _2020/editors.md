---
layout: lecture
title: "Текстовые редакторы (Vim)"
date: 2020-01-15
ready: true
video:
  aspect: 56.25
  id: a6Q8Na575qc
---

Написание слов и написание кода - очень разные активности. При программировании,
вы затрачиваете значительно больше времени на переходы между файлами, чтение, перемещение и
редактирование кода, в сравнении с написанием большого объема информации. Это объясняет тот факт, что существуют разные программы для написания текста и кода (к примеру:
Microsoft Word и Visual Studio Code).

Как программисты, мы тратим основную часть времени на редактирование кода.
Поэтому будет полезно потратить время на совершенствование навыков работы в том редакторе, который соответствует вашим требованиям.

Вот основная концепция изучения и совершенствования работы в новом редакторе:
- Ознакомьтесь с обучающими материалами (пример: эта лекция и указанные в ней дополнительные материалы);
- Начните использовать выбранный редактор для всех случаев редактирования текстовой информации (даже если скорость работы в нем значительно ниже);
- Обращайте внимание на детали при работе: если вам кажется, что есть способ сделать что-то эффективнее, скорее всего такой способ существует.

Если вы будете придерживаться данной концепции, обязуясь использовать новый редактор при любой необходимости, ваш прогресс в изучении сложных текстовых редакторов будет подобен указанному далее.
В течение часа или двух, вы ознакомитесь с базовым функционалом, таким как
открытие, редактирование и сохранение файлов, выход из программы, и использование буфера.
Как только вы перешагнете порог в 20 часов использования - вы будете таким же быстрым в использовании нового редактора, каким были при использовании старого.
После этого ваша эффективность будет возрастать: вы будете иметь достаточно знаний и мышечной памяти и использование нового редактора будет экономить ваше время.
Современные текстовые редакторы - сложные, но изысканные и производительные инструменты, поэтому обучение не должно прекращаться - вы будете использовать их более эффективно при дальнейшем изучении.

# Какой редактор использовать?

Программисты [достаточно предубеждены](https://en.wikipedia.org/wiki/Editor_war)
в отношении выбранного текстового редактора.

Какие из редакторов популярны сегодня? Если обратиться к результатам опроса [Stack Overflow
survey](https://insights.stackoverflow.com/survey/2019/#development-environments-and-tools)
(результаты опроса могут быть предвзяты, т.к. респонденты на одном сайте не представляют всех программистов). [Visual Studio
Code](https://code.visualstudio.com/) наиболее популярный текстовый редактор.
[Vim](https://www.vim.org/) наиболее популярный консольный текстовый редактор.

## Vim

Все лекторы данного курса используют Vim как редактор. Vim имеет богатую историю;
он происходит от редактора Vi (1976), который поддерживается по сей день.
Vim имеет несколько отличительных идей, и благодаря этому,
большое количество других инструментов поддерживают эмуляцию Vim (к примеру,
1.4 миллиона пользователей установили плагин [Vim emulation for VS code](https://github.com/VSCodeVim/Vim)).
Обучиться использовать Vim - стоящее дело, даже если 
в итоге вы будете использовать другой текстовый редактор.  

Невозможно обучить вас всему функционалу Vim в течении 50 минут, поэтому мы сфокусируемся
на объяснении "философии" Vim, расскажем основы, познакомим с некоторыми
продвинутыми функциональностями и поделимся полезной информацией для совершенствования навыков работы.

# Философия Vim

В процессе программирования, основная часть времени тратится на чтение и редактирование,
а не на написание кода. В связи с этим, Vim является редактором с _переключаемыми режимами_ : для ввода текста или манипуляций с текстом требуется переключить режим. Vim - это программируемый редактор
(при использовании Vimscript и других языков, к примеру - Python),
и в тоже время интерфейс Vim по своей сути - язык программирования:
нажатие клавиш воспринимается как команды (по мнемоническим именам), и эти команды - комбинируемые.
В Vim избегается возможность использования мыши, так как это замедляет работу; в Vim даже избегается возможность использовать "стрелки" на клавиатуре для перемещения по тексту -
так как на это требуется лишнее движение.

В конечном результате скорость работы в данном редакторе может соответствовать скорости, с которой вы думаете.

# Режимы работы

Принцип работы в Vim основан на идее, что основная часть времени при программировании тратится на чтение, перемещение по коду и внесение небольших правок, а не на написание больших объемов текста.
В связи с этим в Vim присутствуют несколько режимов работы.

- **Normal**: для перемещения по файлу и внесения правок
- **Insert**: для ввода текста
- **Replace**: для замены текста
- **Visual** (простой, линейный или блочный): для выделения частей текста
- **Command-line**: для выполнения команд

Нажатия клавиш воспринимаются по-разному в разных режимах. К примеру, нажатие клавиши `x` в режиме Insert приведет к вставке символа 'x', но в 
Normal - символ, на котором находится курсор, будет удален, а в Visual - будет удален весь выделенный текст.

В своей конфигурации по-умолчанию режим работы Vim отображается внизу слева.
Изначальный (стандартный) режим - Normal. Большую часть своего времени вы
будете проводить в режимах Normal и Insert.

Нажатие клавиши `<ESC>` используется для переключения в режим Normal из любого другого.
Из режима Normal переход в режим Insert осуществляется при нажатии `i`, в Replace - `R`, в Visual (простой) - `v`, в Visual (линейный) - `V`, Visual (блочный) - `<C-v>` (Ctrl-V, иногда пишется как `^V`), и режим Command-line - `:`.

Нажимать клавишу `<ESC>` придется достаточно часто в Vim: подумайте над перезначением клавиши Caps Lock в
Escape ([macOS
instructions](https://vim.fandom.com/wiki/Map_caps_lock_to_escape_in_macOS)).

# Основы

## Вставка текста (Insert)

Для перехода в режим Insert из режима Normal, требуется нажать клавишу `i`. 
Теперь поведение Vim будет схоже с любым другим текстовым редактором, пока клавиша `<ESC>` не будет нажата для возврата в Normal режим. Это,
с учетом основ, описанных ранее, достаточно для начала работы в Vim (хотя и не особо эффективно, если вы проводите все время при редактировании в режиме Insert).

## Буферы, вкладки и окна

Vim поддерживает открытие нескольких файлов, называемых "буферами". Сессия Vim имеет перечень вкладок, каждая из которых содержит окна (разделенных панелей). Каждое окно отображает один буфер. В отличии от других программ известных вам, к примеру - веб-браузер, в Vim нет строгого соответствия (1 к 1) между буферами и окнами: окна - это просто интерфейс отображения. Выбранный буфер может быть открыт одновременно в _нескольких_ окнах, и даже более того - в нескольких окнах одной вкладки. Это достаточно удобно в случаях необходимости отображения двух разных частей одного файла одновременно.

По умолчанию, Vim открывается с одной вкладкой, содержащей одно окно.

## Командная строка (Command-line)

В режим Command-line можно перейти нажав `:` в режиме Normal. Ваш курсор переместится
в строку команд в низу экрана после нажатия `:`. Этот режим имеет большое количество функциональностей, включающих открытие, сохранение, закрытие файлов и
[выход из Vim](https://twitter.com/iamdevloper/status/435555976687923200).

- `:q` quit (Закрыть окно)
- `:w` save (Сохранить изменения - от англ. "write")
- `:wq` сохранить и выйти
- `:e {name of file}` открыть файл для редактирования - от англ. "edit"
- `:ls` показать открытые буферы - от англ. "list show" 
- `:help {topic}` открыть окно помощи
    - `:help :w` открыть подсказку  для команды `:w`
    - `:help w` открыть подсказку при `w` навигации

# Интерфейс Vim - язык программирования

Наиболее важная идея в Vim заключается в том, что интерфейс Vim является языком программирования.
Вводимые символьные комбинации (из мнемонических имен) - команды, и эти команды можно
_объединять_. Это позволяет эффективно перемещаться и вносить изменения, в особенности тогда, когда команды отложены в мышечной памяти.

## Перемещение

Вы должны проводить основную часть времени в Normal режиме, используя команды для перемещения по
буферу. Перемещения в Vim так же называют "существительные", потому что они относятся на части (блоки) текста.

- Базовое перемещение: `hjkl` (влево, вниз, вверх, вправо)
- Слова: `w` (следующее слово), `b` (начало предыдущего слова), `e` (конец следующего слова )
- Строки: `0` (начало строки), `^` (первый непробельный символ строки), `$` (окончание строки)
- Экран: `H` (верх экрана), `M` (середина экрана), `L` (низ экрана)
- Scroll: `Ctrl-u` (вверх), `Ctrl-d` (вниз)
- Файл: `gg` (начало файла), `G` (конец файла)
- Номер строки	: `:{number}<CR>` либо `{number}G` (номер строки - {number}, <CR> - нажатие клавиши Enter)
- Дополнительно: `%` (связанный символ - пример: переход между скобками, кавычками и т.д.)
- Поиск: `f{character}`, `t{character}`, `F{character}`, `T{character}`	
	- f | F - поиск символа в строке (вперед | назад)
    - t | T - переход к символу, за которым находится искомый {character} в текущей строке (вперед | назад)
    - `,` / `;` для перехода между результатами поиска
- Поиск по файлу: `/{regex}`, `n` / `N` для навигации по результатам.

## Выбор

Визуальный режим:

- Visual
- Visual Line
- Visual Block

После перехода в визуальный режим - используйте клавиши навигации для выбора части текста.

## Внесение изменений

Все, что вы раньше делали с помощью мыши, сейчас требуется делать на клавиатуре с помощью команд редактирования, которые  следует объединить с командами перемещения.
Вот где интерфейс Vim начинает выглядеть как язык программирования. Команды редактирования в Vim так же называют глаголами "verbs", потому что глаголы действуют на существительные.

- `i` переход в Insert режим
    - но для изменения/удаления текст желательно использовать что-то более удобное чем "backspace"
- `o` / `O` вставка строки ниже / Выше
- `d{motion}` удалить {команда передвижения}
    - примеры: `dw` - удалить слово, `d$` - удалить все до конца строки, `d0` - удалить все до начала строки, `d3d` - удалить 3 строки.
- `c{motion}` изменить {команда передвижения}
    - к примеру: `cw` - изменить слово (удалить и перейти в режим Insert)
- `x` удалить символ (соответствует - `dl`)
- `s` заменить символ (соответствует - `xi`)
- Visual режим + манипуляции
    - выделить текст, `d` - для удаления либо `c` для его изменения
- `u` для отмены последнего действия, `<C-r>` - повторение отмененного действия
- `y` копирование
- `p` вставка
- И еще большое количество для изучения, к примеру -  `~` изменяет регистр символа

## Количество

Вы можете объединять существительные и глаголы (объекты и действия) с количеством,в результате чего будет произведено выбранное действие указанное количество раз.

- `3w` переместиться на 3 слова вперед
- `5j` переместиться на 5 строк вниз
- `7dw` удалить 7 слов

## Модификаторы

Вы можете использовать модификаторы для изменения значения существительных (объектов). Некоторые модификаторы являются `i`,
что обозначает - внутренний, внутри ("inner", "inside"), и `a` - внешние ("around").

- `ci(` изменить содержимое внутри текущей пары скобок
- `ci[` изменить содержимое внутри текущей пары квадратных скобок
- `da'` удалить содержимое подстроки, отмеченной одинарными кавычками, включая одинарные кавычки.

# Demo

Here is a broken [fizz buzz](https://en.wikipedia.org/wiki/Fizz_buzz)
implementation:

```python
def fizz_buzz(limit):
    for i in range(limit):
        if i % 3 == 0:
            print('fizz')
        if i % 5 == 0:
            print('fizz')
        if i % 3 and i % 5:
            print(i)

def main():
    fizz_buzz(10)
```

We will fix the following issues:

- Main is never called
- Starts at 0 instead of 1
- Prints "fizz" and "buzz" on separate lines for multiples of 15
- Prints "fizz" for multiples of 5
- Uses a hard-coded argument of 10 instead of taking a command-line argument

{% comment %}
- main is never called
  - `G` end of file
  - `o` open new line below
  - type in "if __name__ ..." thing
- starts at 0 instead of 1
  - search for `/range`
  - `ww` to move forward 2 words
  - `i` to insert text, "1, "
  - `ea` to insert after limit, "+1"
- newline for "fizzbuzz"
  - `jj$i` to insert text at end of line
  - add ", end=''"
  - `jj.` to repeat for second print
  - `jjo` to open line below if
  - add "else: print()"
- fizz fizz
  - `ci'` to change fizz
- command-line argument
  - `ggO` to open above
  - "import sys"
  - `/10`
  - `ci(` to "int(sys.argv[1])"
{% endcomment %}

See the lecture video for the demonstration. Compare how the above changes are
made using Vim to how you might make the same edits using another program.
Notice how very few keystrokes are required in Vim, allowing you to edit at the
speed you think.

# Customizing Vim

Vim is customized through a plain-text configuration file in `~/.vimrc`
(containing Vimscript commands). There are probably lots of basic settings that
you want to turn on.

We are providing a well-documented basic config that you can use as a starting
point. We recommend using this because it fixes some of Vim's quirky default
behavior. **Download our config [here](/2020/files/vimrc) and save it to
`~/.vimrc`.**

Vim is heavily customizable, and it's worth spending time exploring
customization options. You can look at people's dotfiles on GitHub for
inspiration, for example, your instructors' Vim configs
([Anish](https://github.com/anishathalye/dotfiles/blob/master/vimrc),
[Jon](https://github.com/jonhoo/configs/blob/master/editor/.config/nvim/init.vim) (uses [neovim](https://neovim.io/)),
[Jose](https://github.com/JJGO/dotfiles/blob/master/vim/.vimrc)). There are
lots of good blog posts on this topic too. Try not to copy-and-paste people's
full configuration, but read it, understand it, and take what you need.

# Extending Vim

There are tons of plugins for extending Vim. Contrary to outdated advice that
you might find on the internet, you do _not_ need to use a plugin manager for
Vim (since Vim 8.0). Instead, you can use the built-in package management
system. Simply create the directory `~/.vim/pack/vendor/start/`, and put
plugins in there (e.g. via `git clone`).

Here are some of our favorite plugins:

- [ctrlp.vim](https://github.com/ctrlpvim/ctrlp.vim): fuzzy file finder
- [ack.vim](https://github.com/mileszs/ack.vim): code search
- [nerdtree](https://github.com/scrooloose/nerdtree): file explorer
- [vim-easymotion](https://github.com/easymotion/vim-easymotion): magic motions

We're trying to avoid giving an overwhelmingly long list of plugins here. You
can check out the instructors' dotfiles
([Anish](https://github.com/anishathalye/dotfiles),
[Jon](https://github.com/jonhoo/configs),
[Jose](https://github.com/JJGO/dotfiles)) to see what other plugins we use.
Check out [Vim Awesome](https://vimawesome.com/) for more awesome Vim plugins.
There are also tons of blog posts on this topic: just search for "best Vim
plugins".

# Vim-mode in other programs

Many tools support Vim emulation. The quality varies from good to great;
depending on the tool, it may not support the fancier Vim features, but most
cover the basics pretty well.

## Shell

If you're a Bash user, use `set -o vi`. If you use Zsh, `bindkey -v`. For Fish,
`fish_vi_key_bindings`. Additionally, no matter what shell you use, you can
`export EDITOR=vim`. This is the environment variable used to decide which
editor is launched when a program wants to start an editor. For example, `git`
will use this editor for commit messages.

## Readline

Many programs use the [GNU
Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) library for
their command-line interface. Readline supports (basic) Vim emulation too,
which can be enabled by adding the following line to the `~/.inputrc` file:

```
set editing-mode vi
```

With this setting, for example, the Python REPL will support Vim bindings.

## Others

There are even vim keybinding extensions for web
[browsers](http://vim.wikia.com/wiki/Vim_key_bindings_for_web_browsers) - some
popular ones are
[Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb?hl=en)
for Google Chrome and [Tridactyl](https://github.com/tridactyl/tridactyl) for
Firefox. You can even get Vim bindings in [Jupyter
notebooks](https://github.com/lambdalisue/jupyter-vim-binding).

# Advanced Vim

Here are a few examples to show you the power of the editor. We can't teach you
all of these kinds of things, but you'll learn them as you go. A good
heuristic: whenever you're using your editor and you think "there must be a
better way of doing this", there probably is: look it up online.

## Search and replace

`:s` (substitute) command ([documentation](http://vim.wikia.com/wiki/Search_and_replace)).

- `%s/foo/bar/g`
    - replace foo with bar globally in file
- `%s/\[.*\](\(.*\))/\1/g`
    - replace named Markdown links with plain URLs

## Multiple windows

- `:sp` / `:vsp` to split windows
- Can have multiple views of the same buffer.

## Macros

- `q{character}` to start recording a macro in register `{character}`
- `q` to stop recording
- `@{character}` replays the macro
- Macro execution stops on error
- `{number}@{character}` executes a macro {number} times
- Macros can be recursive
    - first clear the macro with `q{character}q`
    - record the macro, with `@{character}` to invoke the macro recursively
    (will be a no-op until recording is complete)
- Example: convert xml to json ([file](/2020/files/example-data.xml))
    - Array of objects with keys "name" / "email"
    - Use a Python program?
    - Use sed / regexes
        - `g/people/d`
        - `%s/<person>/{/g`
        - `%s/<name>\(.*\)<\/name>/"name": "\1",/g`
        - ...
    - Vim commands / macros
        - `Gdd`, `ggdd` delete first and last lines
        - Macro to format a single element (register `e`)
            - Go to line with `<name>`
            - `qe^r"f>s": "<ESC>f<C"<ESC>q`
        - Macro to format a person
            - Go to line with `<person>`
            - `qpS{<ESC>j@eA,<ESC>j@ejS},<ESC>q`
        - Macro to format a person and go to the next person
            - Go to line with `<person>`
            - `qq@pjq`
        - Execute macro until end of file
            - `999@q`
        - Manually remove last `,` and add `[` and `]` delimiters

# Resources

- `vimtutor` is a tutorial that comes installed with Vim - if Vim is installed, you should be able to run `vimtutor` from your shell
- [Vim Adventures](https://vim-adventures.com/) is a game to learn Vim
- [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
- [Vim Advent Calendar](https://vimways.org/2019/) has various Vim tips
- [Vim Golf](http://www.vimgolf.com/) is [code golf](https://en.wikipedia.org/wiki/Code_golf), but where the programming language is Vim's UI
- [Vi/Vim Stack Exchange](https://vi.stackexchange.com/)
- [Vim Screencasts](http://vimcasts.org/)
- [Practical Vim](https://pragprog.com/titles/dnvim2/) (book)

# Exercises

1. Complete `vimtutor`. Note: it looks best in a
   [80x24](https://en.wikipedia.org/wiki/VT100) (80 columns by 24 lines)
   terminal window.
1. Download our [basic vimrc](/2020/files/vimrc) and save it to `~/.vimrc`. Read
   through the well-commented file (using Vim!), and observe how Vim looks and
   behaves slightly differently with the new config.
1. Install and configure a plugin:
   [ctrlp.vim](https://github.com/ctrlpvim/ctrlp.vim).
   1. Create the plugins directory with `mkdir -p ~/.vim/pack/vendor/start`
   1. Download the plugin: `cd ~/.vim/pack/vendor/start; git clone
      https://github.com/ctrlpvim/ctrlp.vim`
   1. Read the
      [documentation](https://github.com/ctrlpvim/ctrlp.vim/blob/master/readme.md)
      for the plugin. Try using CtrlP to locate a file by navigating to a
      project directory, opening Vim, and using the Vim command-line to start
      `:CtrlP`.
    1. Customize CtrlP by adding
       [configuration](https://github.com/ctrlpvim/ctrlp.vim/blob/master/readme.md#basic-options)
       to your `~/.vimrc` to open CtrlP by pressing Ctrl-P.
1. To practice using Vim, re-do the [Demo](#demo) from lecture on your own
   machine.
1. Use Vim for _all_ your text editing for the next month. Whenever something
   seems inefficient, or when you think "there must be a better way", try
   Googling it, there probably is. If you get stuck, come to office hours or
   send us an email.
1. Configure your other tools to use Vim bindings (see instructions above).
1. Further customize your `~/.vimrc` and install more plugins.
1. (Advanced) Convert XML to JSON ([example file](/2020/files/example-data.xml))
   using Vim macros. Try to do this on your own, but you can look at the
   [macros](#macros) section above if you get stuck.
