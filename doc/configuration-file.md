# Configuration file

The configuration file is **optional**. PHP-DI will work with default behavior without it.

Here is an example of a configuration file (`di.php`):

```php
// Dependency injection configuration
Container::addConfiguration(array(

	// Used to restrict the configuration to a specific namespace
	"namespace" => "",

	// Value injections
	"values" => array(
		"db.params" => array(
			"driver"   => "pdo_mysql",
			"user"     => "root",
			"password" => "",
			"dbname"   => "foo",
		),
		"model" => true,
		"isDevelopment" => true,
	),

	// Type mapping for injection using abstract types
	"mapping" => array(
		"\My\Interface"     => "\My\Implementation",
		"\My\AbstractClass" => "\My\OtherImplementation",
	),

	// Explicit bean definition
	"beans" => array(
		"entityManager" => function(Container $c) {
			$config = Doctrine\ORM\Tools\Setup::createAnnotationMetadataConfiguration(
													APPLICATION_PATH . "/models", $c["isDevelopment"]);
			return Doctrine\ORM\EntityManager::create($c["db.params"], $config);
		},
	),

));
```

To import the configuration file:

```php
\DI\Container::getInstance()->addConfigurationFile('di.ini');
```

## Value injection

```php
Container::addConfiguration(array(
	"values" => array(
		"key" => "value",
	),
));
```

Defines a value that can be injected using the [@Value annotation](doc/value-annotation).

## Type mapping

```php
Container::addConfiguration(array(
	"mapping" => array(
		"\My\Interface" => "\My\Implementation",
	),
));
```

If you are trying to inject an abstract type (interface or abstract class),
this configuration allows you to define which implementation to use.

You can also use it to map any types, i.e. to override a type being injected.