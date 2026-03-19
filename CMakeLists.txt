cmake_minimum_required(VERSION 3.16)

project(Sourcebreaker VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# --- 1. ВКЛЮЧАЕМ АВТОМАТИКУ QT ---
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

# --- 2. ИСПРАВЛЯЕМ КИРИЛЛИЦУ В ВЫВОДЕ ---
if(MSVC)
    add_compile_options(/utf-8)
endif()

# --- 3. ПУТИ К БИБЛИОТЕКАМ ---
list(APPEND CMAKE_PREFIX_PATH
    "C:/Qt/6.10.2/msvc2022_64"
    "D:/Libs/KDDockWidgets"
    "C:/Program Files/LLVM"
)

# --- 4. ПОИСК ПАКЕТОВ ---
find_package(Qt6 REQUIRED COMPONENTS Widgets Sql)
find_package(KDDockWidgets-qt6 REQUIRED)

find_path(CLANG_INCLUDE_DIR clang-c/Index.h PATHS "C:/Program Files/LLVM/include")
find_library(CLANG_LIBRARY NAMES libclang PATHS "C:/Program Files/LLVM/lib")

# --- 5. СОЗДАНИЕ ИСПОЛНЯЕМОГО ФАЙЛА ---
add_executable(Sourcebreaker
    main.cpp
    mainwindow.cpp
    mainwindow.h
    mainwindow.ui  # Файл формы должен быть в списке!
)

target_include_directories(Sourcebreaker PRIVATE ${CLANG_INCLUDE_DIR})

# --- 6. ЛИНКОВКА ---
target_link_libraries(Sourcebreaker PRIVATE
    Qt6::Widgets
    Qt6::Sql
    KDAB::kddockwidgets  # Попробуйте без -qt6, если не найдет - верните -qt6
    ${CLANG_LIBRARY}
)

qt_finalize_executable(Sourcebreaker)