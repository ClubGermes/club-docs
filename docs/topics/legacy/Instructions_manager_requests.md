# Назначение встреч

- К1 - Клиент первый (инициатор)
  - М1 - Менеджер первого клиента
- К2 – Клиент второй 
  - М2 - Менеджер второго клиента

```mermaid
graph TD
  A[Инициализация встречи в приложении К1] -->|Уведомление М1 и М2| B{Согласование встречи M1, M2}
  B -->|М1 и М2 подтвердили запрос встречи К1| C{Согласование встречи К2 в приложении}
  B -->|М1 или М2 выбирает новое время встречи| D[Новые слоты через календарь]
  B -->|Встреча не нужна| E[Запрос встречи<br/> удален]
  C -->|К2 подтверждает встречу| F[Встреча назначена]
  C -->|К2 не устраивает время| B
  C -->|К2 отказался от встречи| B
  D -->|Уведомление К1 и К2| F
```

## Инициализация встречи в приложении К1
```mermaid
  sequenceDiagram autonumber
  box rgba(33,66,99,0.01) 1
    actor Customer1 as Клиент №1 <br/> "К1"
    actor Manager1 as Менеджер №1 <br/> "M1"
  end
  participant ClubGermes as Приложение <br/> "Гермес клуб"
  participant ClubManager as ЛК менеджера <br/> "Светофор"
  box rgba(33,66,99,0.01) 2
    actor Manager2 as Менеджер №2 <br/> "M2"
    actor Customer2 as Клиент №2 <br/> "К2"
  end
  
  Customer1 ->>+ ClubGermes: ИНИЦИАЛИЗАЦИЯ ВСТРЕЧИ В ПРИЛОЖЕНИИ
  Customer1 ->>+ ClubGermes: Ввод: описания встречи
  Customer1 ->>+ ClubGermes: Выбор: слоты
  par У М1 и М2 появляются оповещения
    ClubManager ->> Manager1: Монитор: "Онлайн": карточка Клиента1
    ClubManager ->> Manager2: Монитор: "Онлайн": карточка Клиента2
    ClubManager ->> Manager1: Монитор:<br/>Уведомления (колокольчик): сообщение
    ClubManager ->> Manager2: Монитор:<br/>Уведомления (колокольчик): сообщение
    ClubManager ->> Manager1: Онлайн-запросы: новая встреча
    ClubManager ->> Manager2: Онлайн-запросы: новая встреча
    ClubManager ->> Manager1: Email: сообщение
    ClubManager ->> Manager2: Email: сообщение
  end
  note over Customer1,Customer2: Согласование встречи М1 и М2
```
## Согласование встречи М1 и М2
```mermaid
  sequenceDiagram autonumber
  box rgba(33,66,99,0.01) 1
    actor Customer1 as Клиент №1 <br/> "К1"
    actor Manager1 as Менеджер №1 <br/> "M1"
  end
  participant ClubGermes as Приложение <br/> "Гермес клуб"
  participant ClubManager as ЛК менеджера <br/> "Светофор"
  box rgba(33,66,99,0.01) 1
    actor Manager2 as Менеджер №2 <br/> "M2"
    actor Customer2 as Клиент №2 <br/> "К2"
  end
  alt Встреча не нужна
    Manager1 -->> Customer1: Уточняет информацию
    Manager1 ->> Manager1: Решает что встреча не нужна
    Manager1 ->> ClubManager: Удалить запрос
    ClubGermes ->> Customer1: Карточка встречи исчезает
    break
        note over Customer1,Manager1: СТОП
    end
  else Встреча нужна
    Manager1 -->> Customer1: Уточняет информацию
    Manager1 ->> Manager1: Решает что встреча нужна
    Manager1 ->> ClubManager: Нажимает "Контроль"
    Manager1 -> Manager2: Согласовывают время встречи
    Manager2 -->> Customer2: Уточняет информацию
    Manager2 ->> ClubManager: Нажимает "Контроль"
    alt или подтверждение встречи
      Manager1 ->> ClubManager: подтверждает встречу онлайн
      Manager2 ->> ClubManager: подтверждает встречу онлайн
      note over Customer1,Customer2: Оповещения: M1 и M2 подтвердили встречу
    else или М1 назначает встречу через календарь
        Manager1 ->> ClubManager: выбирает новое время встречи
        note over Customer1,Customer2: Оповещения: М1 или М2 выбрал новое время встречи
    else или М2 назначает встречу через календарь
        Manager2 ->> ClubManager: выбирает новое время встречи
        note over Customer1,Customer2: Оповещения: М1 или М2 выбрал новое время встречи
    end 
  end
```

## M1 или М2 выбрал новое время встречи
```mermaid
  sequenceDiagram autonumber
  box rgba(33,66,99,0.01) 1
    actor Customer1 as Клиент №1 <br/> "К1"
    actor Manager1 as Менеджер №1 <br/> "M1"
  end
  participant ClubGermes as Приложение <br/> "Гермес клуб"
  participant ClubManager as ЛК менеджера <br/> "Светофор"
  box rgba(33,66,99,0.01) 1
    actor Manager2 as Менеджер №2 <br/> "M2"
    actor Customer2 as Клиент №2 <br/> "К2"
  end
    par У М1 и М2 (пропадает)
      ClubManager ->> Manager1: Онлайн-запросы: новая встреча пропадает
      ClubManager ->> Manager2: Онлайн-запросы: новая встреча пропадает
      ClubManager ->> Manager1: Монитор: "Онлайн": карточка Клиента 1 пропадает
      ClubManager ->> Manager2: Монитор: "Онлайн": карточка Клиента 2 пропадает
    end
    par У М1 и М2 (появляется)
      ClubManager ->> Manager1: Уведомления (колокольчик): сообщение
      ClubManager ->> Manager2: Уведомления (колокольчик): сообщение
      ClubManager ->> Manager1: Email: сообщение
      ClubManager ->> Manager2: Email: сообщение
      ClubManager ->> Manager1: Онлайн-встречи: новая встреча
      ClubManager ->> Manager2: Онлайн-встречи: новая встреча
    end
    par У К1 и К2 (появляется)
      ClubGermes ->> Customer1: Push-сообщение что назначена встреча
      ClubGermes ->> Customer2: Push-сообщение что назначена встреча
      Customer1 --> Customer2: ссылка в Push ведет в раздел "Назначенные"
      ClubGermes ->> Customer1: Email: сообщение
      ClubGermes ->> Customer2: Email: сообщение
    end
    break
      note over Customer1,Customer2: СТОП
    end
```

## M1 и M2 подтвердили запрос встречи К1
```mermaid
  sequenceDiagram autonumber
  box rgba(33,66,99,0.01) 1
    actor Customer1 as Клиент №1 <br/> "К1"
    actor Manager1 as Менеджер №1 <br/> "M1"
  end
  participant ClubGermes as Приложение <br/> "Гермес клуб"
  participant ClubManager as ЛК менеджера <br/> "Светофор"
  box rgba(33,66,99,0.01) 1
    actor Manager2 as Менеджер №2 <br/> "M2"
    actor Customer2 as Клиент №2 <br/> "К2"
  end
      par Оповещения
      ClubGermes ->> Customer1: Push-сообщение что предложена встреча
      ClubGermes ->> Customer2: Push-сообщение что предложена встреча
      Customer1 --> Customer2: ссылка в Push ведет в раздел "Запрошенные"
      ClubGermes ->> Customer1: Email: сообщение что предложена встреча
      ClubGermes ->> Customer2: Email: сообщение что предложена встреча
      ClubManager ->> Manager1: Уведомления (колокольчик): сообщение
      ClubManager ->> Manager2: Уведомления (колокольчик): сообщение
      ClubGermes ->> Customer2: Запрошенные: индикация и выбор
    end
    note over Customer1,Customer2: Согласование встречи К2 в приложении
```

## Согласование встречи К2 в приложении
```mermaid
  sequenceDiagram autonumber
  box rgba(33,66,99,0.01) 1
    actor Customer1 as Клиент №1 <br/> "К1"
    actor Manager1 as Менеджер №1 <br/> "M1"
  end
  participant ClubGermes as Приложение <br/> "Гермес клуб"
  participant ClubManager as ЛК менеджера <br/> "Светофор"
  box rgba(33,66,99,0.01) 1
    actor Manager2 as Менеджер №2 <br/> "M2"
    actor Customer2 as Клиент №2 <br/> "К2"
  end
  
  alt или подтверждение встречи
    Customer2 ->> ClubGermes: Подтверждает временной слот
    ClubGermes ->> Customer1: Push-сообщение<br/>"К2 принял ваше предложение о встрече ... время и дата"
    Customer1 --> Customer2: ссылка в Push ведет в раздел "Назначенные"
    ClubGermes ->> Customer1: Email: сообщение
    ClubManager ->> Manager1: Монитор: Уведомления (колокольчик): сообщение<br/>"К2 принял предложение К1 о встрече ... время и дата."
    ClubManager ->> Manager2: Монитор: Уведомления (колокольчик): сообщение<br/>"К2 принял предложение К1 о встрече ... время и датаи."
    Manager1 --> Manager2: Само сообщение выделить зеленым цветом.
    ClubManager ->> Manager1: Email: сообщение "К2 принял предложение К1 о встрече ... время и дата."
    ClubManager ->> Manager2: Email: сообщение "К2 принял предложение К1 о встрече ... время и дата."
    par У М1 и М2 (пропадает)
      ClubManager ->> Manager1: Онлайн-запросы: новая встреча пропадает
      ClubManager ->> Manager2: Онлайн-запросы: новая встреча пропадает
      ClubManager ->> Manager1: Монитор: "Онлайн": карточка Клиента 1 пропадает
      ClubManager ->> Manager2: Монитор: "Онлайн": карточка Клиента 2 пропадает
    end
    par У М1 и М2 (появляется)
      ClubManager ->> Manager1: Онлайн-встречи: новая встреча
      ClubManager ->> Manager2: Онлайн-встречи: новая встреча
    end
    par У К1 и К2 (появляется)
      ClubGermes ->> Customer1: Встречи\Назначенные: Карточка встречи К2
      ClubGermes ->> Customer2: Встречи\Назначенные: Карточка встречи К1
      ClubGermes ->> Customer1: Push-сообщение что назначена встреча
      ClubGermes ->> Customer2: Push-сообщение что назначена встреча
      Customer1 --> Customer2: ссылка в Push ведет в раздел "Назначенные"
      ClubGermes ->> Customer1: Email: сообщение
      ClubGermes ->> Customer2: Email: сообщение
    end
    break
      note over Customer1,Customer2: СТОП
    end
  else или не удобно время встречи
      Customer2 ->> ClubGermes: Не удобно время встречи
      par У М1 и М2 не пропадают оповещения
        ClubManager ->> Manager1: Онлайн-запросы: новая встреча
        ClubManager ->> Manager2: Онлайн-запросы: новая встреча
        ClubManager ->> Manager1: Монитор: "Онлайн": карточка Клиента1
        ClubManager ->> Manager2: Монитор: "Онлайн": карточка Клиента2
      end
      par У М1 и М2 добавляются оповещения
        ClubManager ->> Manager1: Монитор: Уведомления (колокольчик):<br/>сообщение "К2 не согласовал с К1 время встречи.<br/>Требуется мнимание КМ"
        ClubManager ->> Manager2: Монитор: Уведомления (колокольчик):<br/>сообщение "К2 не согласовал с К1 время встречи.<br/>Требуется мнимание КМ"
        Manager1 --> Manager2: Само сообщение выделить желтым цветом.
        ClubManager ->> Manager1: Email: сообщение "К2 не согласовал с К1 время встречи.<br/>Требуется мнимание КМ"
        ClubManager ->> Manager2: Email: сообщение "К2 не согласовал с К1 время встречи.<br/>Требуется мнимание КМ"
        ClubManager ->> Manager1: Онлайн-запросы: Требуется переназначение времени
        ClubManager ->> Manager2: Онлайн-запросы: Требуется переназначение времени
      end
      note over Customer1,Customer2: М1 или М2 назначает встречу через календарь. <br/> Оповещения: M1 или М2 выбрал новое время встречи
  else или встреча не нужна
    Customer2 ->> ClubGermes: Отклоняет встречу
    par У М1 и М2 не пропадают оповещения
      ClubManager ->> Manager1: Онлайн-запросы: новая встреча
      ClubManager ->> Manager2: Онлайн-запросы: новая встреча
      ClubManager ->> Manager1: Монитор: "Онлайн": карточка Клиента1
      ClubManager ->> Manager2: Монитор: "Онлайн": карточка Клиента2
    end
    par У М1 и М2 добавляются оповещения
      ClubManager ->> Manager1: Монитор: Уведомления (колокольчик):<br/>сообщение "К2 отказал К1 во встрече.<br/>Требуется мнимание КМ"
      ClubManager ->> Manager2: Монитор: Уведомления (колокольчик):<br/>сообщение "К2 отказал К1 во встрече.<br/>Требуется мнимание КМ"
      Manager1 --> Manager2: Само сообщение выделить красным цветом.
      ClubManager ->> Manager1: Email: сообщение "К2 отказал К1 во встрече.<br/>Требуется мнимание КМ"
      ClubManager ->> Manager2: Email: сообщение "К2 отказал К1 во встрече.<br/>Требуется мнимание КМ"
      ClubManager ->> Manager1: Онлайн-запросы: отказ
      ClubManager ->> Manager2: Онлайн-запросы: отказ
    end
    note over Customer1,Customer2: М1 или М2 назначает встречу через календарь. <br/> Оповещения: M1 или М2 выбрал новое время встречи
  end
```