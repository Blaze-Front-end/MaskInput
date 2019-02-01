# Для создания маски мобильного телефона необходимо:

### 1)	создать input, добавить в него следующие атрибуты: 
1.1 id="phone2"
1.2 type="tel"
1.3 required или required="" (без разницы)
1.4 pattern="[\+]\d{1}\s[\(]\d{3}[\)]\s\d{3}[\-]\d{2}[\-]\d{2}" (проверяет средствами css правильное заполнение формы P.S не использовать в Pug)
1.5 minlength="18" (проверка средствами css заполнено ли поле полностью)
1.6 maxlength="18" (проверка средствами css не ввел ли пользователь больше символов)
1.7 остальные параметры вроде class name placeholder при необходимости

 
На изображении пример такого input. Для создание нескольких input на странице требуется указывать все те же атрибуты, только поменять id.
### 2)	Добавить следующий скрипт на страницу (либо тегом script в конец странице, либо в отельный файл со скриптами).
<script>
    window.addEventListener("DOMContentLoaded", function() {
        function setCursorPosition(pos, elem) {
            elem.focus();
            if (elem.setSelectionRange) elem.setSelectionRange(pos, pos);
        else if (elem.createTextRange) {
                var range = elem.createTextRange();
                range.collapse(true);
                range.moveEnd("character", pos);
                range.moveStart("character", pos);
                range.select()
            }
        }
        function mask(event) {
            var matrix = "+7 (___) ___-__-__",
            i = 0,
            def = matrix.replace(/\D/g, ""),
            val = this.value.replace(/\D/g, "");
            if (def.length >= val.length) val = def;
            this.value = matrix.replace(/./g, function(a) {
                return /[_\d]/.test(a) && i < val.length ? val.charAt(i++) : i >= val.length ? "" : a
            });
            if (event.type == "blur") {
                if (this.value.length == 2) this.value = ""
            } else setCursorPosition(this.value.length, this)
        };
        let input = document.querySelector("#phone"),
            input1 = document.querySelector("#phone2");
        input.addEventListener("input", mask, false);
        input.addEventListener("focus", mask, false);
        input.addEventListener("blur", mask, false);
        input1.addEventListener("input", mask, false);
        input1.addEventListener("focus", mask, false);
        input1.addEventListener("blur", mask, false);
    });
</script>


### 3)	Добавить стили
При создании input с атрибутом required на браузерах мозилла возникает проблема отображения красной рамки вокруг такого input после загрузки страницы. Решить эту проблему можно добавив следующий код в стили:

input:required {
    box-shadow: none;
}
input:invalid {
    box-shadow: 2px red;
}
или вообще отключив эту рамку:
input[required=""]{
    box-shadow: none;
}
