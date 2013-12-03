jsRusTextToDate (AngularJS)
===============

Convert russian text to date (http://jsfiddle.net/dRRb5/)

Html body:
```html
<body ng-app>
    <div ng-controller="MainCtrl">
    Введите текст даты: 
        <br/>
        <input ng-model="data.myinput"/><br/>
        <span>Распознано как:</span> <b>{{dt = jsParseDate(data.myinput); dt.title}}</b><br/>
        <span>Дата:</span> <b>{{dt.date.toLocaleString()}}</b><br/>
        <hr>
            Кликните для проверки примеров:
            <ol>
                <li ng-repeat="example in data.examples" ng-click="data.myinput=example.title">{{example.title}}</a></li>
                   
            </ol>
    </div
</body>
```

Javascript:
```js
function MainCtrl($scope) {
    $scope.data = {};
    $scope.data.answer = "56";
    $scope.data.myinput = "Напомнить про ДР жены через 10 дней в 11:30";
    $scope.data.examples = [
        {title: "Мыть машину через 2 дня в 11"},
        {title: "Поздравить с ДР через год в 12:30"},
        {title: "Записаться на пятницу"},
        {title: "Проснуться 2 января"},
        {title: "Через 50 лет купить котедж"},
        {title: "Сегодня в 16 позвонить жене"},
        {title: "Через пять дней в 14:00"},
        {title: "Поздравить всех с НГ 31 декабря в 23"},
        {title: "Получить письмо в 18"},
        {title: "Послезавтра в 11 приехать за зарплатой"},
        {title: "Сегодня в 21часов 30 минут выключить Дом2 "},
        {title: "Купить браслет Jawbone Up через 2 дня в 20:00"},
        {title: "Напомнить сделать рефакторинг в субботу"},
        {title: "Перезагрузить сервер в воскресение в 2"}
    ]

//парсер даты (позвонить послезавтра)
$scope.jsParseDate = function(title) {
        title = title.replace(" один", " 1").replace(" один", " 1").replace(" два", " 2").replace(" три", " 3").replace(" четыре", " 4").replace(" пять", " 5").replace(" шесть", " 6").replace(" семь", " 7").replace("восемь", "8").replace("девять", "9").replace("десять", "10").replace("одинадцать", "11").replace("двенадцать", "12").replace("тринадцать", "13").replace("четырнадцать", "14").replace("пятнадцать", "15").replace(" шестнадцать", " 16").replace(" семнадцать", " 17").replace(" двадцать", " 20").replace(" ноль", " 0");
        if (title) title = title.toLowerCase();
        var answer = "";
        var did = false;
        var mytime = "";
        var mydate = new Date();
        var newdate = new Date();
        var d = new Object;
        d.myhours = 0;
        d.myminutes = 0;
        d.mydays = 0;
        d.mymonth = 0;
        d.myyears = 0;
        d.myweek = 0;
        var shablon = /(\d{1,2}.\d{1,2}.\d{4})/g;
        var matches = title.match(shablon);
        if (matches) {
            shablon = /(\d{1,4})/g;
            var matches2 = matches[0].match(shablon);
            newdate.setDate(matches2[0]);
            newdate.setMonth(matches2[1] - 1);
            newdate.setFullYear(matches2[2]);
            answer = matches2[0] + "." + matches2[1] + "." + matches2[2];
            did = true;
        }
        shablon = /(\d{1,2} янв)|(\d{1,2} фев)|(\d{1,2} мар)|(\d{1,2} апр)|(\d{1,2} мая)|(\d{1,2} май)|(\d{1,2} июн)|(\d{1,2} июл)|(\d{1,2} авг)|(\d{1,2} сен)|(\d{1,2} окт)|(\d{1,2} ноя)|(\d{1,2} дек)/g;
        matches = title.match(shablon);
        if (matches) {
            shablon = /(\d{4})/g;
            matches2 = title.match(shablon); //найти год
            shablon = /(янв)|(фев)|(мар)|(апр)|(мая)|(май)|(июн)|(июл)|(авг)|(сен)|(окт)|(ноя)|(дек)/g;
            matches3 = title.match(shablon); //найти месяц
            shablon = /(\d{1,2})/g;
            matches4 = matches[0].match(shablon); //найти дату
            if (matches3[0] == "янв") var mymonth = 1;
            if (matches3[0] == "фев") var mymonth = 2;
            if (matches3[0] == "мар") var mymonth = 3;
            if (matches3[0] == "апр") var mymonth = 4;
            if (matches3[0] == "мая") var mymonth = 5;
            if (matches3[0] == "май") var mymonth = 5;
            if (matches3[0] == "июн") var mymonth = 6;
            if (matches3[0] == "июл") var mymonth = 7;
            if (matches3[0] == "авг") var mymonth = 8;
            if (matches3[0] == "сен") var mymonth = 9;
            if (matches3[0] == "окт") var mymonth = 10;
            if (matches3[0] == "ноя") var mymonth = 11;
            if (matches3[0] == "дек") var mymonth = 12;
            newdate.setDate(matches4[0]);
            newdate.setMonth(mymonth - 1);
            if (matches2) newdate.setFullYear(matches2[0]);
            answer = matches4[0] + " " + matches3[0];
            did = true;
        }
        shablon = /(вчера)|(позавчера)|(сегодня)|(завтра)|(послезавтра)/g;
        matches = title.match(shablon);
        if (matches) {
            if (matches[0] == "позавчера") var add_days = -2;
            if (matches[0] == "вчера") var add_days = -1;
            if (matches[0] == "сегодня") var add_days = 0;
            if (matches[0] == "завтра") var add_days = +1;
            if (matches[0] == "послезавтра") var add_days = +2;
            newdate.setDate(newdate.getDate() + add_days);
            answer = " + " + matches[0];
            did = true;
        }
        shablon = /(\d{1,2}ч|\d{1,2} ч)|(в \d{1,2}:\d{1,2})|(в\d{1,2}:\d{1,2})|(\d{2} ми)|(\d{2}ми)|(\d{1,2} \d{2}м)|(в \d{1,2})|(в\d{1,2})|(\d{1,2}:\d{1,2})/g;
        matches = title.match(shablon);
        if (matches) {
            if (matches.length == 1) {
                mytime = matches;
            } else {
                mytime = matches.join(" ");
            }
        }
        var matches2 = title.match(/\d{1,4}/g); //все двух-значные цифры
        var plus;
        shablon = /(дней|лет|нед|год|мес|день|дня|час|мин|\d{1,2}м|\d{1,2} м)/g;
        matches = title.match(shablon);
        //если "через 2 часа 30 минут"
        if (((title.indexOf("назад") != -1) || (title.indexOf("через") != -1)) && matches) {
            if (title.indexOf("через") != -1) {
                plus = "+";
            } else {
                plus = "-";
            }    
            if (matches[0] == "час") //если указаны часы и минуты
            {
                if (matches2) {
                    answer = plus;
                    if (matches2[0]) {
                        answer += matches2[0] + " час.";
                        d.myhours = plus + matches2[0];
                    }
                    if (matches2[1]) {
                        answer += " " + matches2[1] + " мин.";
                        d.myminutes = plus + matches2[0];
                    }
                    mytime = ""; //это не время
                }
            }
            if (matches[0] == "мин" || (matches[0][matches[0].length - 1] == "м" && (title.indexOf("мес") == -1))) //если указаны только минуты
            {
                if (matches2) {
                    answer = plus;
                    if (matches2[0]) {
                        answer += " " + matches2[0] + " minute";
                        d.myminutes = plus + matches2[0];
                    }
                    mytime = ""; //это не время
                }
            }
            if (matches[0] == "нед") //если указаны только недели
            {
                if (matches2) {
                    answer = plus;
                    if (matches2[0]) {
                        answer += "" + matches2[0] + " нед.";
                        d.myweek = plus + matches2[0];
                    };
                }
                if (title.indexOf("через нед") != -1) {
                    answer = "+ 1 нед.";
                    d.myweek = plus + 1
                };
            }
            if (title.indexOf("месяц") != -1) //если указаны только месяцы
            {
                if (matches2) {
                    answer = plus;
                    if (matches2[0]) {
                        answer += "" + matches2[0] + " мес.";
                        d.mymonth = plus + matches2[0];
                    };
                }
                if (title.indexOf("через мес") != -1) {
                    answer = "+ 1 мес.";
                    d.mymonth = plus + 1;
                };
            }
            if ((title.indexOf(" год") != -1) || (title.indexOf(" лет") != -1)) //если указаны только месяцы
            {
                if (matches2) {
                    answer = plus;
                    if (matches2[0]) {
                        answer += "" + matches2[0] + " год.";
                        d.myyears = plus + matches2[0];
                    };
                }
                if (title.indexOf("через год") != -1) {
                    answer = "+ 1 год.";
                    d.myyears = plus + 1;
                };
            }
            if ((title.indexOf(" день") != -1) || (title.indexOf(" дня") != -1) || (title.indexOf(" дней") != -1)) //если указаны только месяцы
            {
                if (matches2) {
                    answer = plus;
                    if (matches2[0]) {
                        answer += "" + matches2[0] + " дн.";
                        d.mydays = plus + matches2[0];
                    };
                }
                if (title.indexOf("через год") != -1) {
                    answer = "+ 1 дн.";
                    d.mydays = plus + 1;
                };
            }
        }
        if (mytime != "") {
            ///анализ времени
            shablon = /(в \d{1,2})|(в\d{1,2})|(\d{1,2}:\d{1,2})/g;
            matches = mytime.toString().match(shablon);
            if ((matches)) {
                need_analyse = mytime.toString().match(/(в \d{1,2} в \d{1,2})|(\d{1,2} \d{1,2}м)|(\d{1,2}ч\d{1,2}м)|(\d{1,2}ч \d{1,2}м)|(\d{1,2}:\d{1,2})/g);
                shablon1 = /(в \d{1,2}:\d{1,2})|(в\d{1,2}:\d{1,2})/g;
                matches1 = mytime.toString().match(shablon1);
                if (matches1) need_analyse = false;
                if (!need_analyse) {
                    mytime = mytime.toString().replace("в ", "").replace("в", "");
                    if (!matches1) mytime += ":00";
                } else {
                    matches3 = mytime.toString().match(/\d{1,4}/g); //все двух-значные цифры
                    if (matches3) if (matches3.length == 1) mytime = matches3;
                    else mytime = matches3.join(":");
                }
            }
        }
        if (mytime != "") var add = "[" + mytime + "]";
        else var add = "";
        if ((mytime != "")) {
            if (mytime.toString().match(/\d{1,2}:\d{1,2}/g)) {
                newtime = mytime.toString().split(":");
                mydate.setHours(parseInt(newtime[0]), 10);
                mydate.setMinutes(parseInt(newtime[1], 10));
                mydate.setSeconds(0);
            } else {
                mytime = "";
            }
        }
        if (did) {
            newdate.setHours(mydate.getHours() + parseInt(d.myhours, 10));
            newdate.setMinutes(mydate.getMinutes() + parseInt(d.myminutes, 10));
            newdate.setSeconds(0);
            mydate = newdate;
        } else {
            mydate.setHours(mydate.getHours() + parseInt(d.myhours, 10));
            mydate.setMinutes(mydate.getMinutes() + parseInt(d.myminutes, 10));
            mydate.setSeconds(0);
        }
        mydate.setDate(mydate.getDate() + parseInt(d.mydays, 10) + parseInt(d.myweek * 7, 10));
        mydate.setMonth(mydate.getMonth() + parseInt(d.mymonth, 10));
        mydate.setYear(mydate.getFullYear() + parseInt(d.myyears, 10));
        shablon = /(понед)|(вторн)|(сред)|(четв)|(пятн)|(субб)|(воскр)/g;
        matches = title.match(shablon);
        if (matches) {
            week = 0;
            if (matches[0] == "понед") week = 1;
            if (matches[0] == "вторн") week = 2;
            if (matches[0] == "сред") week = 3;
            if (matches[0] == "четв") week = 4;
            if (matches[0] == "пятн") week = 5;
            if (matches[0] == "субб") week = 6;
            if (matches[0] == "воскр") week = 7;
            if (week != 0) {
                mydate = nextWeekDay(mydate, week);
                answer = matches[0];
            }
        }
        if ((answer == "") && (mytime == "")) mydate = "";
        if (title.toLowerCase().indexOf("смс") != -1 || title.toLowerCase().indexOf("sms") != -1 || title.toLowerCase().indexOf("напомни") != -1) {
            var remind_time_default = localStorage.getItem("remind_time");
            remind_time = remind_time_default ? remind_time_default : 15;
            add += " | Напомнить за " + remind_time + " м";
            var sms = " | Напомнить за " + remind_time + " м";
        } else {
            var sms = "";
        }
        return {
            title: answer + " " + add,
            date: mydate,
            sms: sms
        };
    } //jsParseDate

function nextWeekDay(date, day) { //поиск следующего дня недели
    (day = (Math.abs(+day || 0) % 7) - date.getDay()) < 0 && (day += 7);
    return day && date.setDate(date.getDate() + day), date;
};
    
}
```

```css
input {
    width: 90%;
    font-size: 20px;
    padding: 5px 5px;
}
span {
    display: inline-block;
    width: 120px;
    text-align: right;
    color: #888;
}
span {
    margin: 5px;
}

li {
    cursor: pointer;
    padding: 2px;
}
```
