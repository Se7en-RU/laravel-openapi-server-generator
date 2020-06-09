# Laravel OpenApi Server Generator

Пакет для Laravel, который генерирует Dto модели при помощи [OpenApi Generator](https://openapi-generator.tech/).

## Зависимости:
1. Java 8 и выше.

## Установка:
1. `composer require --dev greensight/laravel-openapi-server-generator`
2. `npm install --save-dev @openapitools/openapi-generator-cli` - установка npm обертки для бинарника openapi-generator
3. `php artisan vendor:publish --provider="Greensight\LaravelOpenapiServerGenerator\OpenapiServerGeneratorServiceProvider"` - копирует конфиг генератора в конфиги приложения

## Запуск:
Перед запуском убедиться, что структура описания апи соответствует [этим требованиям]().

Запускать командой: `php artisan openapi:generate-server`

После успешного выполнения в директории `app/<appDir> (указывается в конфиге)` должны появиться следующие файлы:
1. Dto - директория со всеми Dto апи;
2. Enums - директория с перечислениями, которые используются в апи;
3. ObjectSerializer.php и Configuration.php - вспомогательные файлы для Dto;

### Важно:
После первой генерации в файле ObjectSerializer.php в функции sanitizeForSerialization нужно добавить следующие строки:
```php
if ($value !== null) {
    $values[$data::attributeMap()[$property]] = self::sanitizeForSerialization($value, $openAPIType, $formats[$property]);
} else { // новые строки, который нужно добавить
    $values[$data::attributeMap()[$property]] = null;
}
