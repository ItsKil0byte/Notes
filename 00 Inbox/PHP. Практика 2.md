- Объекты интерфейсы и всё такое
- Классы всегда передаются по ссылке
- clone $obj - склонировать объект
- new stdClass(); - создание нового объекта
- stdClass - родительский класс PHP
- Скобочки можно опускать при отсутсвии параметров
- unset() - удаление
- $obj::class - узнать класс объекта
- $obj instanceof Class

- \[final] \[abstract] class ИмяКласса \[extend Parent] \[instance Interface] { }
- Множественного наследования нет
- Атрибуты класса:
	- public int $num = 0;
	- protected string $name = "";
	- private bool $key = false;
- Магические методы:
	- public function \_\_construct(): void { }
	- public function \_\_destruct(): void { }
	- public function \_\_get(string $name): mixed { }
		- Автоматически вызывается при попытке чтения данных из недоступных (protected/private) или несуществующих свойств объекта
	- public function \_\_set(string $name, mixed $value): void { }
	- public function \_\_isset(string $name): bool { }

- interface Name extends Parent
	- public function method(): mixed;

- Traits:
	- ...

- Перечисления:

```php
enum NameEnum 
{
	case val1 = 'string';
	
	public function value(): string
	{
		return match($this)
		{
			self::val1=>'val1',
			default=>'unknown'		
		};
	}
}

$e = NameEnum::val1;
echo $e->value();
echo $e->value;
```

- Типизированные enum
- Всегда уникальные значения

Праааактика

- Обработка исключений
```php
try { ... }
catch(Exception $e) { throw new RuntimeException("Ошибка", 0, $e) }
finally { ... }
```