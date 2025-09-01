# total_duration_v2.0

## EN

This script completes a homework assignment to calculate the total duration of video files in directories. It finds all .mp4 files and outputs the total duration for each folder separately, as well as the total duration for all videos.

[**Link: Bash script `total_duration_v1.1`**](https://github.com/DShJr/total_duration_v1.1)

### Assignment Execution Process

**Script Name:** `total_duration_v2.0.sh`

```Bash
#!/bin/bash

# Find all .mp4 files, get their duration,
# group them by folder, and output the result.

find . -name "*.mp4" -print0 | while IFS= read -r -d '' file; do
    duration=$(ffprobe -v error -show_entries format=duration -of default=noprin
t_wrappers=1:nokey=1 "$file")
    if [ -n "$duration" ]; then
        dir=$(dirname "$file")
        echo "$dir $duration"
    fi
done | awk '
{
    dir_sums[$1] += $2
    total_sum += $2
}
END {
    for (dir in dir_sums) {
        s = dir_sums[dir]
        h = int(s/3600)
        m = int((s%3600)/60)
        sec = int(s%60)
        printf "%-30s: %02d:%02d:%02d\n", dir, h, m, sec
    }

    print "------------------------------"

    s = total_sum
    h = int(s/3600)
    m = int((s%3600)/60)
    sec = int(s%60)
    printf "%-30s: %02d:%02d:%02d\n", "ИТОГО", h, m, sec
}'
```

#### **Script Logic:**

The script consists of two key stages connected by a **pipeline** (`|`).

#### **Stage 1: Data Collection**

The first part of the script is responsible for finding and extracting the necessary data.

* The `find` command locates all files with the `.mp4` extension in the current directory and all its subfolders. The `-print0` option makes the output safe, allowing the script to handle file names that contain spaces or special characters correctly. 
* Next, the output of the `find` command is passed to a `while read` loop. This loop processes each found file line by line.
* For each file, the `ffprobe` utility extracts its duration in seconds.
* Finally, the folder path and the duration of each file are printed on a single line.

#### **Stage 2: Grouping and Output**

The data collected in the first stage is passed to the second block of the script, the `awk` utility.

* `awk` uses an **associative array** (`dir_sums`), where the key is the folder path, and the value is the total duration of all videos in that folder.
* As it processes each line, `awk` adds the duration of the current file to the corresponding value in the array.
* After all lines have been processed (the `END` block), `awk` iterates through the entire array, formats the duration for each folder into `hh:mm:ss`, and prints the result. 
* Finally, the script outputs a separator and the total duration of all video files.



---
## RU

Этот скрипт выполняет задание по подсчёту общей продолжительности видеофайлов в директориях. Он находит все файлы .mp4 и выводит суммарную продолжительность для каждой папки отдельно, а также общую продолжительность по всем видео.

[**Ссылка: Bash script `total_duration_v1.1`**](https://github.com/DShJr/total_duration_v1.1)

### Процесс выполнение задания

**Имя скрипта:** `total_duration_v2.0.sh`

```Bash
#!/bin/bash

# Найти все .mp4 файлы, получить их продолжительность,
# сгруппировать по папкам и вывести результат.

find . -name "*.mp4" -print0 | while IFS= read -r -d '' file; do
    duration=$(ffprobe -v error -show_entries format=duration -of default=noprin
t_wrappers=1:nokey=1 "$file")
    if [ -n "$duration" ]; then
        dir=$(dirname "$file")
        echo "$dir $duration"
    fi
done | awk '
{
    dir_sums[$1] += $2
    total_sum += $2
}
END {
    for (dir in dir_sums) {
        s = dir_sums[dir]
        h = int(s/3600)
        m = int((s%3600)/60)
        sec = int(s%60)
        printf "%-30s: %02d:%02d:%02d\n", dir, h, m, sec
    }

    print "------------------------------"

    s = total_sum
    h = int(s/3600)
    m = int((s%3600)/60)
    sec = int(s%60)
    printf "%-30s: %02d:%02d:%02d\n", "ИТОГО", h, m, sec
}'
```

#### **Логика работы скрипта:**

Скрипт состоит из двух ключевых этапов, которые соединены с помощью **пайпа** (`|`).

#### **Этап 1: Сбор данных**

Первая часть скрипта отвечает за поиск и извлечение необходимых данных.

* Команда `find` находит все файлы с расширением `.mp4` в текущей директории и всех её подпапках. Опция `-print0` делает результат безопасным, позволяя скрипту корректно работать даже с именами файлов, содержащими пробелы или специальные символы. 
* Далее, вывод команды `find` передаётся в цикл `while read`. Этот цикл построчно обрабатывает каждый найденный файл.
* Для каждого файла утилита `ffprobe` извлекает его продолжительность в секундах.
* Наконец, путь к папке и продолжительность каждого файла выводятся на одной строке.

#### **Этап 2: Группировка и вывод**

Данные, собранные на первом этапе, передаются во второй блок скрипта — утилиту `awk`.

* `awk` использует **ассоциативный массив** (`dir_sums`), где ключом является путь к папке, а значением — общая продолжительность всех видео в этой папке.
* По мере обработки строк `awk` добавляет продолжительность каждого файла к соответствующему значению в массиве.
* После того как все строки обработаны (блок `END`), `awk` проходит по всему массиву, форматирует продолжительность для каждой папки в формат `чч:мм:сс` и выводит результат. 
* В конце скрипт выводит разделитель и общую продолжительность всех видеофайлов.
