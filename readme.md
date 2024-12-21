# Урок 1. Dom-дерево

## Вы разрабатываете веб-страницу для отображения расписания занятий в спортивном клубе. Каждое занятие имеет название, время проведения, максимальное количество участников и текущее количество записанных участников.

1. Создайте веб-страницу с заголовком "Расписание занятий" и областью для отображения занятий.

2. Загрузите информацию о занятиях из предоставленных JSON-данных. Каждое занятие должно отображаться на странице с указанием его названия, времени проведения, максимального количества участников и текущего количества записанных участников.

3. Пользователь может нажать на кнопку "Записаться" для записи на занятие. Если максимальное количество участников уже достигнуто, кнопка "Записаться" становится неактивной.

4. После успешной записи пользователя на занятие, обновите количество записанных участников и состояние кнопки "Записаться".

5. Запись пользователя на занятие можно отменить путем нажатия на кнопку "Отменить запись". После отмены записи, обновите количество записанных участников и состояние кнопки.

6. Все изменения (запись, отмена записи) должны сохраняться и отображаться в реальном времени на странице.

7. При разработке используйте Bootstrap для стилизации элементов.

---

## Решение:


index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    <title>Расписание занятий</title>
</head>
<body>
    <div class="content container d-flex flex-wrap justify-content-center align-items-start py-5 gap-3"></div>
    <script src="index.js"></script>
</body>
</html>
```

---
index.js

```
const lessons = [
    {
        name: 'Физика',
        startTime: '2023-08-17T08:30:00.000Z',
        maxNumberOfParticipants: 30,
        curNumberOfParticipants: 17
    },
    {
        name: 'Химия',
        startTime: '2023-08-17T10:00:00.000Z',
        maxNumberOfParticipants: 30,
        curNumberOfParticipants: 19
    },
    {
        name: 'Математика',
        startTime: '2023-08-17T11:30:00.000Z',
        maxNumberOfParticipants: 35,
        curNumberOfParticipants: 25
    },
    {
        name: 'Геометрия',
        startTime: '2023-08-17T13:00:00.000Z',
        maxNumberOfParticipants: 35,
        curNumberOfParticipants: 26
    },
    {
        name: 'Биология',
        startTime: '2023-08-17T14:30:00.000Z',
        maxNumberOfParticipants: 32,
        curNumberOfParticipants: 11
    },
    {
        name: 'Черчение',
        startTime: '2023-08-17T16:00:00.000Z',
        maxNumberOfParticipants: 25,
        curNumberOfParticipants: 22
    },
    {
        name: 'Астрономия',
        startTime: '2023-08-17T17:30:00.000Z',
        maxNumberOfParticipants: 20,
        curNumberOfParticipants: 15
    },
    {
        name: 'История',
        startTime: '2023-08-17T19:00:00.000Z',
        maxNumberOfParticipants: 20,
        curNumberOfParticipants: 15
    }
];

const contentEl = document.querySelector('.content');

function createLessonHTML(rootEl, lessonData) {
    const lessonDiv = document.createElement('div');
    lessonDiv.classList.add('card');
    lessonDiv.classList.add('card-body');
    lessonDiv.classList.add('shadow-sm');

    const lessonName = document.createElement('h2');
    lessonName.textContent = lessonData.name;
    lessonName.classList.add('lesson-name');

    const lessonTime = document.createElement('p');
    lessonTime.textContent = 'Дата начала: ' + lessonData.startTime;
    lessonTime.classList.add('lesson-time');

    const lessonMaxPartisipants = document.createElement('p');
    lessonMaxPartisipants.innerHTML = 'Максимальное количество участников: ' + `<span>${lessonData.maxNumberOfParticipants}</span>`;
    lessonMaxPartisipants.classList.add('max-participants');

    const lessonCurPartisipants = document.createElement('p');
    lessonCurPartisipants.innerHTML = 'Текущее количество участников: ' + `<span>${lessonData.curNumberOfParticipants}</span>`;
    lessonCurPartisipants.classList.add('cur-participants');

    const subscribeBtn = document.createElement('button');
    subscribeBtn.textContent = 'Записаться';
    subscribeBtn.classList.add('subscribe');
    subscribeBtn.classList.add('btn');
    subscribeBtn.classList.add('btn-primary');
    subscribeBtn.classList.add('align-items-center');

    const unSubscribeBtn = document.createElement('button');
    unSubscribeBtn.textContent = 'Отменить запись';
    unSubscribeBtn.classList.add('unsubscribe');
    unSubscribeBtn.classList.add('btn');
    unSubscribeBtn.classList.add('btn-secondary');
    unSubscribeBtn.classList.add('align-items-center');

    lessonDiv.append(lessonName);
    lessonDiv.append(lessonTime);
    lessonDiv.append(lessonMaxPartisipants);
    lessonDiv.append(lessonCurPartisipants);
    lessonDiv.append(subscribeBtn);
    lessonDiv.append(unSubscribeBtn);
    rootEl.append(lessonDiv);
}

window.addEventListener('load', () => {
    lessons.forEach(element => {
        createLessonHTML(contentEl, element);
    });
});

contentEl.addEventListener('click', e => {
    const target = e.target;
    
    // Нажата кнопка записаться или кнопка отписаться
    if (target.classList.contains('subscribe') || target.classList.contains('unsubscribe')) {
        // Найти ближайщий div в который входит кнопка
        const parentDiv = target.closest('div');

        const subscribeBtn = parentDiv.querySelector('.subscribe');
        const unSubscribeBtn = parentDiv.querySelector('.unsubscribe');

        const curParticipants = parentDiv.querySelector('.cur-participants span');
        const maxParticipants = parentDiv.querySelector('.max-participants span');

        let curParticipantsValue = Number(curParticipants.textContent);
        let maxParticipantsValue = Number(maxParticipants.textContent);

        // Нажата кнопка записаться
        if (target.classList.contains('subscribe')) {
            if (curParticipantsValue < maxParticipantsValue) {
                curParticipants.textContent = ++curParticipantsValue;
            }
        }
        // Нажата кнопка отписаться
        if (target.classList.contains('unsubscribe')) {
            if (curParticipantsValue > 0) {
                curParticipants.textContent = --curParticipantsValue;
            }
        }

        curParticipantsValue < maxParticipantsValue ? subscribeBtn.removeAttribute('disabled') : subscribeBtn.setAttribute('disabled', '');

        curParticipantsValue > 0 ? unSubscribeBtn.removeAttribute('disabled') : unSubscribeBtn.setAttribute('disabled', '');

    }
});
```