# Монитор

---

![Монитор](legacy/manager_home_screens/home.png){ border-effect="line" width="406"}

<table>
<tr><td>Блок</td><td>Описание</td></tr>
<tr><td><img src="legacy/manager_home_ico/stadia_1_control_blue.png" alt="stadia_1_control_blue" width="189"/></td><td>2</td></tr>
<tr><td><img src="legacy/manager_home_ico/stadia_2_online_yellow.png" alt="stadia_2_online_yellow" width="189"/></td><td>5</td></tr>

[//]: # (<tr><td><img src="stadia_3_aspirant_green.png" alt="stadia_3_aspirant_green" width="189"/></td><td>5</td></tr>)
<tr><td><img src="legacy/manager_home_ico/stadia_4_admission_green.png" alt="stadia_4_admission_green" width="189"/></td><td>5</td></tr>
<tr><td><img src="legacy/manager_home_ico/stadia_5_connection_green.png" alt="stadia_5_connection_green" width="189"/></td><td>5</td></tr>
<tr><td><img src="legacy/manager_home_ico/stadia_6_candidate_green.png" alt="stadia_6_candidate_green" width="189"/></td><td>5</td></tr>
<tr><td><img src="legacy/manager_home_ico/stadia_7_payment_green.png" alt="stadia_7_payment_green" width="189"/></td><td>5</td></tr>
<tr><td><img src="legacy/manager_home_ico/stadia_8_member_green.png" alt="stadia_8_member_green" width="189"/></td><td>5</td></tr>

[//]: # (<tr><td><img src="stadia_9_aspirant_red.png" alt="stadia_9_aspirant_red" width="189"/></td><td>5</td></tr>)
<tr><td><img src="legacy/manager_home_ico/stadia_10_admission_red.png" alt="stadia_10_admission_red" width="189"/></td><td>5</td></tr>

[//]: # (<tr><td><img src="stadia_11_connection_red.png" alt="stadia_11_connection_red" width="189"/></td><td>5</td></tr>)
[//]: # (<tr><td><img src="stadia_12_candidate_red.png" alt="stadia_12_candidate_red" width="189"/></td><td>5</td></tr>)
</table>

<br>
<warning>Блоки не будут показаны если хоть один из фильтров установлен в true: Просрочка (delay), Отложен (post), Отказ (reject)
    <p>и соответствующий массив данных пуст.</p>
</warning>
<table>
<tr><td>Блок</td><td>Описание</td><td><warning>И блоки не будут показаны если:</warning></td></tr>
<tr><td>Возврат агенту</td><td>5</td>
<td></td></tr>
<tr><td>Переведенные клиенты</td><td>5</td>
    <td>
        <p>У пользователя нет роли 'community manager'.</p>
        <p>Стадия У агента (stage0).</p>
    </td>
</tr>
</table>

<warning>Блоки не будут показаны если у пользователя нет одной из ролей: 'admin', 'manager', 'moderator', 'chief', 'community manager'
    <p>и соответствующий массив данных пуст.</p>
</warning>
<table>
<tr><td>Блок</td><td>Описание</td><td><warning>И блоки не будут показаны если:</warning></td></tr>
<tr><td>Возврат от агента</td><td>5</td>
<td></td></tr>
<tr><td>Контрольное время</td><td>5</td>
    <td>Стадия У агента (stage0).</td>
</tr>
</table>

<warning>Блоки не будут показаны если хоть один из фильтров установлен в true: Просрочка (delay), Отложен (post), Отказ (reject).
    <p>и если у пользователя нет одной из ролей: 'admin', 'manager', 'moderator', 'chief', 'community manager',</p>
    <p>и стадия У агента (stage0),</p>
    <p>и соответствующий массив данных пуст.</p>
</warning>
<table>
<tr><td>Блок</td><td>Описание</td></tr>
<tr><td>Регистрации на мероприятия</td><td>5</td></tr>
<tr><td>Онлайн-запросы</td><td>5</td></tr>
</table>

<br>

<procedure title="Фильтры 1" id="filters1">
<p></p>
    <step>
        <p>Менеджеры - Отобразить только клиентов определенного менеджера.</p>
        <img src="legacy/manager_home_ico/select_manager.png" alt="select_agent" width="189"/>
    </step>
    <step>
        <p>Агенты - Отобразить только клиентов определенного агента.</p> 
        <img src="legacy/manager_home_ico/select_agent.png" alt="select_agent" width="189"/>
    </step>
    <step>
        <p>Дата комментария - Отобразить только клиентов в карте которых оставлен комментарий.</p> 
        <img src="legacy/manager_home_ico/select_comments.png" alt="select_agent" width="189"/>
        <list>
            <li value="-">Не учитывать комментарии</li>
            <li value="a">Есть за последние 3 дня</li>
            <li value="b">Есть за последнюю неделю</li>
            <li value="c">Есть за последние две недели</li>
            <li value="d">Нет 3 дня</li>
            <li value="e">Нет неделю</li>
            <li value="f">Нет две недели</li>
        </list>
    </step>
</procedure>

<procedure title="Фильтры 2" id="filters2">
    Счетчик клиентов: Всего / 30 дней / 7 дней / Вчера
        <p></p>
        <img src="legacy/manager_home_ico/new_clients.png" alt="new_clients" width="189"/>

</procedure>

<procedure title="Фильтры 3" id="filters3">
<p>Базовое состояние фильтров:</p>
    <img src="legacy/manager_home_ico/filters_base.png" alt="base filters condition" border-effect="line"/>
    <step>
        <p>Контрольный вопрос (control_questions)</p>
        <list>
            <li>Не учитывать <img src="legacy/manager_home_ico/1.png" alt="filter" width="32"/></li>
            <li>Учитывать только записи, где control_questions установлено <img src="legacy/manager_home_ico/1.1.png" alt="filter" width="32"/></li>
            <li>Учитывать только записи, где control_questions отсутствует <img src="legacy/manager_home_ico/1.2.png" alt="filter" width="32"/></li>
        </list>
    </step>
    <step>
        <p>Рейтинг (удовлетворенности???) участника в клубе (membership.rating)</p>
        Вот <img src="legacy/manager_home_ico/2.png" alt="filter" width="32"/> / <img src="legacy/manager_home_ico/2.1.png" alt="filter" width="32"/>, 
        потом чего-то еще <img src="legacy/manager_home_ico/3.png" alt="filter" width="32"/> / <img src="legacy/manager_home_ico/3.1.png" alt="filter" width="32"/>,
        и так <img src="legacy/manager_home_ico/4.png" alt="filter" width="32"/> / <img src="legacy/manager_home_ico/4.1.png" alt="filter" width="32"/>.
        <p> Такие же иконки фильтра на аватарках клиентов в списках.</p>         
        <warning>В настоящее время не используется, так как всем клиентам присвоен 3-й рейтинг.</warning>
    </step>
    <step>
        <p>Просрочка. (delay) </p>
        <p><img src="legacy/manager_home_ico/5.png" alt="filter" width="32"/> Если filter.post, filter.reject и filter.stage0 равны false</p>
        <p><img src="legacy/manager_home_ico/5.1.png" alt="filter" width="32"/> Если хотя бы одно из значений filter.post, filter.reject или filter.stage0 равно true</p>
    </step>    
    <step>
        <p>У агента. (stage0) отображения только в том случае, если переменная ifAgentOnly равна false</p>
        <p><img src="legacy/manager_home_ico/6.png" alt="filter" width="32"/> Если filter.delay, filter.reject и filter.post равны false</p>
        <p><img src="legacy/manager_home_ico/6.1.png" alt="filter" width="32"/> Если хотя бы одно из значений filter.delay, filter.reject или filter.post равно true</p>
    </step>
    <step>
        <p>Отложен. (post) </p>
        <p><img src="legacy/manager_home_ico/7.png" alt="filter" width="32"/> Если filter.delay, filter.reject равны false, а filter.stage0 равно true</p>
        <p><img src="legacy/manager_home_ico/7.1.png" alt="filter" width="32"/> Если хотя бы одно из значений filter.delay, filter.reject или filter.stage0 равно true</p>
    </step>
    <step>        
        <p>Отказ (reject) </p>
        <p><img src="legacy/manager_home_ico/8.png" alt="filter" width="32"/> Если filter.delay, filter.post и filter.stage0 равны false</p>
        <p><img src="legacy/manager_home_ico/8.1.png" alt="filter" width="32"/> Если хотя бы одно из значений filter.delay, filter.post или filter.stage0 равно true</p>
    </step>
    <step>
        <p>filter.cmFlag </p>
        True <img src="legacy/manager_home_ico/9.png" alt="filter" width="32"/> False <img src="legacy/manager_home_ico/9.1.png" alt="filter" width="32"/>,
    </step>
    <step>
        <p>filter.opFlag </p>
        True <img src="legacy/manager_home_ico/10.png" alt="filter" width="32"/> False <img src="legacy/manager_home_ico/10.1.png" alt="filter" width="32"/>,
    </step>
    <step>
        <p>filter.noneFlag </p>
        True <img src="legacy/manager_home_ico/11.png" alt="filter" width="32"/> False <img src="legacy/manager_home_ico/11.1.png" alt="filter" width="32"/>.
    </step>
    <step>
        <p>Такие:</p>
        Вот <img src="legacy/manager_home_ico/12.png" alt="filter" width="32"/> / <img src="legacy/manager_home_ico/12.1.png" alt="filter" width="32"/> / <img src="legacy/manager_home_ico/12.2.png" alt="filter" width="32"/>
        что-то это да значит.
    </step>
</procedure>
