[![CMake CI](https://github.com/Wartheree/lab04/actions/workflows/ci.yml/badge.svg)](https://github.com/Wartheree/lab04/actions/workflows/ci.yml)
## Отчёт к lab04
В рамках выполнения данной лабораторной работы вместо Travis CI, мною был использован Github actions, все команды из tutorial были адаптированы под него
1) Скопирован репозиторий из lab03:
```bash
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04
Клонирование в «projects/lab04»...
remote: Enumerating objects: 31, done.
remote: Counting objects: 100% (31/31), done.
remote: Compressing objects: 100% (21/21), done.
Получение объектов: 100% (31/31), 8.29 КиБ | 652.00 КиБ/с, готово.
Определение изменений: 100% (6/6), готово.
remote: Total 31 (delta 6), reused 27 (delta 5), pack-reused 0 (from 0)
$ cd projects/lab04
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04.git
```
2) Создан конфигурационный файл ci.yml:
```bash
$ cat github/workflows/ci.yml
name: CMake CI

on:
  push:
    branches: [ master, main ]
  pull_request:
    branches: [ master, main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Install CMake
      run: |
        sudo apt-get update
        sudo apt-get install -y cmake cmake-data
    
    - name: Configure CMake
      run: cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
    
    - name: Build
      run: cmake --build _build
    
    - name: Install
      run: cmake --build _build --target install

```

3) Этот файл запушен в данный репозиторий (в первый раз код был написан ошибкой, поэтому комментарий к коммиту "fixed CI":
```bash
$ git add .github/workflows/ci.yml
$ git commit -m "fixed CI"
```
4) В резьтате выполнения в репозитории была найдена ошибка в print.сpp, которая была впоследствии исправлена:
   <img width="2175" height="858" alt="image_2026-04-19_18-27-08" src="https://github.com/user-attachments/assets/a76abaef-b034-4cfb-9289-b62cfec10e0a" />

5) В данный README.md добавлен бейдж (находится в самом начале).



