# Сборка ядра Linux

### Подготовка

- Убедимся, что все необходимые программы установлены в системе ([ссылка](https://www.kernel.org/doc/html/latest/process/changes.html#));

> [!IMPORTANT]  
> Очень важно установить `grub`, чтобы при запуске системы можно было выбрать, какое из установленных ядер запускать.

- Склонируем официальный репозиторий с Linux 6.11 ([ссылка](https://github.com/torvalds/linux/tree/98f7e32f20d28ec452afb208f9cffc08448a2652));
- Выведем текущую версию ядра и архитектуру:

    ![uname](/img/6.5.0.jpg)

### Сборка и установка

- `make defconfig` - создадим дефолтную конфигурацию ядра;
- `make menuconfig` - в открывшемся окне изменим необходимые параметры ядра. Для этого проверим, что в сборку включены драйверы для подключения к сети, bluetooth, вывода аудио, поддержки nvme и т.д. Также исключим из сборки большое количество ненужных драйверов;
- Итоговый конфигурационный файл можно посмотреть, перейдя в [/config](https://github.com/EltexEmbeddedC/kernel-building/blob/main/config/.config);
- `sudo make -j<число ядер> bzImage` - соберем ядро;

    ![сборка ядра](/img/bzImage.jpg)
- `sudo make -j<число ядер> modules` - соберем модули ядра;

    ![сборка модулей](/img/modules.jpg)
- Теперь установим всё это в систему:
    - `sudo make modules_install`

        ![установка модулей](/img/modules_install.jpg)

    - `sudo make install`

        ![установка](/img/install.jpg)

### Тестирование

- Перезапустим систему;
- В открывшемся окне `grub` выберем новую версию ядра;
- После входа в систему выведем текущую версию ядра и архитектуру:

    ![uname](/img/6.11.0.jpg)

    > Образ ядра с "-dirty" означает, что изменения, сделанные в коде ядра перед компиляцией, не были зафиксированы. В данном случае переменной `EXTRAVERSION` в `Makefile` было присвоено значение `eltex_task`, и это изменение не было зафиксировано в git.
- Проверим работу основных систем: интернета, bluetooth, воспроизведения аудио;
- После успешного прохождения этих этапов ядро можно считать успешно установленным.
