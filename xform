(function () {
    const script = document.currentScript || 
        document.querySelector('script[src*="xform.js"]') ||
        (function () {
            const scripts = document.getElementsByTagName('script');
            return scripts[scripts.length - 1];
        })();

    const params = {
        sid:      script.getAttribute('sid')      || '111111111',
        country:  script.getAttribute('country')  || 'AAAAAAAAA',
        sub1:     script.getAttribute('sub1')     || '',
        sub2:     script.getAttribute('sub2')     || '',
        lang:     script.getAttribute('lang')     || '',
        x_id:     script.getAttribute('x_id')     || '',
        fb_pixel: script.getAttribute('fb_pixel') || ''
    };

    document.querySelectorAll('form').forEach((form, index) => {
        // Помогаем идентифицировать форму
        const formId    = form.id ? `#${form.id}` : '';
        const formClass = form.className ? form.className.trim().split(/\s+/).join('.') : '';
        const formLabel = formId || formClass || `форма #${index + 1}`;

        // Проверяем наличие обязательных полей
        const hasName  = !!form.querySelector('input[name="name"]');
        const hasPhone = !!form.querySelector('input[name="phone"]');

        const isValid = hasName && hasPhone;

        // Иконка и цвет в заголовке группы
        const statusIcon  = isValid ? '✅' : '❌';
        const statusText  = isValid ? 'OK' : 'ОШИБКА';
        const groupTitle  = `${statusIcon} Форма ${index + 1}  ${formLabel}  — ${statusText}`;

        console.groupCollapsed(groupTitle);

        // 1. Удаляем старые hidden
        form.querySelectorAll('input[type="hidden"]').forEach(el => el.remove());

        // 2. action + method
        form.action = '../order.php';
        form.method = 'post';

        // 3. Более читаемый вывод полей
        console.log(`name="name"   → ${hasName  ? 'найдено ✓' : 'ОТСУТСТВУЕТ ✗'}`);
        console.log(`name="phone"  → ${hasPhone ? 'найдено ✓' : 'ОТСУТСТВУЕТ ✗'}`);

        if (!isValid) {
            console.warn('Форма НЕ БУДЕТ ОТПРАВЛЯТЬСЯ');
            console.log('Проверьте в этой форме:');
            console.log('  • input с name="name"');
            console.log('  • input с name="phone"');
        }

        // 4. Блокировка отправки при ошибке
        if (!isValid) {
            form.addEventListener('submit', e => {
                e.preventDefault();
                console.error(`Отправка заблокирована в форме: ${formLabel}`);
            });
        }
        // 5. Добавляем скрытые поля только если всё ок
        else {
            const fields = [
                { name: 'fb_pixel', value: params.fb_pixel },
                { name: 'sub1',     value: params.sub1     },
                { name: 'sub2',     value: params.sub2     },
                { name: 'sid',      value: params.sid      },
                { name: 'country',  value: params.country  },
                { name: 'lang',     value: params.lang     },
                { name: 'x_id',     value: params.x_id     }
            ];

            fields.forEach(f => {
                if (!f.value) return;
                const input = document.createElement('input');
                input.type = 'hidden';
                input.name = f.name;
                input.value = f.value;
                form.appendChild(input);
            });

            console.log('Скрытые поля добавлены');
        }

        console.groupEnd();
    });
})();
