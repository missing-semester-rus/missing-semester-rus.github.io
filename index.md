---
layout: page
title: Пропущенный семестр вашего CS-образования
---

Другие курсы затрагивают продвинутые темы в рамках компьютерных наук (CS): от операционных систем до машинного обучения. Но есть один
важный вопрос, который редко освещается – изучение необходимых утилит. Его обычно оставляют студентам для
самостоятельного изучения. Но на этом курсе вы научитесь работать с командной строкой, использовать мощный текстовый редактор,
необычные функции систем контроля версий и многое другое!

Прочитайте о [мотивации создания этого курса](/about/).

# Лекции 2026

<ul>
{% assign lectures = site['2026'] | sort: 'date' %}
{% for lecture in lectures %}
    {% if lecture.phony != true %}
        <li>
        <strong>{{ lecture.date | date: '%-m/%d' }}</strong>:
        {% if lecture.ready %}
            <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% else %}
            {{ lecture.title }} {% if lecture.noclass %}[no class]{% endif %}
        {% endif %}
        </li>
    {% endif %}
{% endfor %}
</ul>

# Лекции 2020

<ul>
{% assign lectures = site['2020'] | sort: 'date' %}
{% for lecture in lectures %}
    {% if lecture.phony != true %}
        <li>
        <strong>{{ lecture.date | date: '%-m/%d' }}</strong>:
        {% if lecture.ready %}
            <a href="{{ lecture.url }}">{{ lecture.title }}</a>
        {% else %}
            {{ lecture.title }} {% if lecture.noclass %}[no class]{% endif %}
        {% endif %}
        </li>
    {% endif %}
{% endfor %}
</ul>

Плейлист с лекциями доступен [на YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J).

# О курсе

**Преподаватели**: [Anish](https://www.anishathalye.com/), [Jon](https://thesquareplanet.com/) и [Jose](http://josejg.com/).

**Перевод 2026**: [Kirill V](https://kirillvasilev.com)

## Благодарности

Авторы курса выражают благодарность Elaine Mello, Jim Cain и
[MIT Open Learning](https://openlearning.mit.edu/) за предоставленную возможность записывать видео с лекциями;
Anthony Zolnik и [MIT AeroAstro](https://aeroastro.mit.edu/) за аудио- и видео оборудование;
и Brandi Adams и [MIT EECS](https://www.eecs.mit.edu/).

---

<div class="small center">
<p><a href="https://github.com/missing-semester-rus/missing-semester-rus.github.io">Курс на русском</a>.</p>
<p>Лицензия CC BY-NC-SA.</p>
<p>Ознакомиться <a href="/license/">по ссылке</a>.</p>
</div>
