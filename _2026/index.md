---
layout: page
title: "Лекции 2026"
description: >
  Конспекты и видео лекций курса Missing Semester, MIT IAP 2026.
permalink: /2026/
phony: true
---

<ul class="double-spaced">
  {% assign lectures = site['2026'] | sort: 'date' %}
  {% for lecture in lectures %}
    {% if lecture.phony != true %}
      <li>
        <strong>{{ lecture.date | date: '%-d.%-m' }}</strong>:
        {% if lecture.ready %}
          <a href="{{ lecture.url}}">{{ lecture.title }}</a>
        {% elsif lecture.noclass %}
          {{ lecture.title }} [занятия нет]
        {% else %}
          {{ lecture.title }} [скоро]
        {% endif %}
        {% if lecture.details %}
          <br>
          ({{ lecture.details }})
        {% endif %}
      </li>
    {% endif %}
  {% endfor %}
</ul>

Видеозаписи лекций доступны [на YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L).

# За пределами MIT

Мы делимся этим курсом и за пределами MIT в надежде, что эти материалы пригодятся и другим. Публикации и обсуждения можно найти на

- [Hacker News](https://news.ycombinator.com/item?id=47124171)
- [Lobsters](https://lobste.rs/s/q4ykw7/missing_semester_your_cs_education_2026)
- [r/learnprogramming](https://www.reddit.com/r/learnprogramming/comments/1r93yk6/the_missing_semester_of_your_cs_education_2026/)
- [X](https://x.com/anishathalye/status/2024521145777848588)
- [Bluesky](https://bsky.app/profile/jonhoo.eu/post/3mfa2bhyuj22i)
- [Mastodon](https://fosstodon.org/@jonhoo/116098318361854057)
- [LinkedIn](https://www.linkedin.com/posts/anishathalye_i-returned-to-mit-during-iap-january-term-activity-7430285026933522433-Ehr9)
- [YouTube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQunmnnTXrNbZnBaCA-ieK4L)

# Благодарности

Мы благодарим Elaine Mello и [MIT Open Learning](https://openlearning.mit.edu/) за возможность записать видео лекций. Мы благодарим Luis Turino / [SIPB](https://sipb.mit.edu/) за поддержку курса в рамках [SIPB IAP 2026](https://sipb.mit.edu/iap/).
