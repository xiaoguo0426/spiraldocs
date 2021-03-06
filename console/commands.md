# Console - User Commands
You can add new console commands to your application or register plugin commands using bootloaders. By default, Console
component configured to automatically find commands in `app/src` directory.

To add new `symfony/console` based command simply drop it into your application. 

## Command class
To create a new command you can either extend `Symfony\Component\Console\Command\Command` or `Spiral\Console\Command` which 
provides some syntax sugar.

```php
use Spiral\Console\Command;

class MyCommand extends Command
{
    const NAME        = 'my:command';
    const DESCRIPTION = 'This is my command';

    const ATTRIBUTES = [];
    const OPTIONS    = [];

    /**
     * Perform command
     */
    protected function perform()
    {
    }
}
```

Use constants `NAME` and `DESCRIPTION` to give a name to your command. You can invoke it now using:

```bash
$ php app.php my:command 
```

## Arguments and Options
Spiral's `Command` class makes easy to define needed arguments and options:

```php
const ARGUMENTS = [
    ['argument', InputArgument::REQUIRED, 'Argument name.']
];
    
const OPTIONS = [
    ['option', 'c', InputOption::VALUE_NONE, 'Some option.']
];
```

To get user's data via arguments and/or options, you can make use of `$this->argument("argName")` or 
`$this->option("optName")`. 

## Perform method
You can put your user code into the `perform` method. The `perform` support method injection and provide `$this->input` and
 `$this->output` properties to work with user input.

```php
protected function perform(MyService $service)
{
    $this->output->writeln($service->doSomething());
}
```

## Helper Methods
You can use a set of helper methods available inside the `Spiral\Console\Command`. Given examples are intended to be called in
the `perform` method.

To write into output:

```php
$this->writeln("hello world");
```

To write into output without advancing to the new line:

```php
$this->write("hello world");
```

To write formatted output without advancing to the new line:

```php
$this->sprintf("Hello, <comment>%s</comment>", $name);
```

> This method is compatible with `sprintf` definition.

To check if current verbosity mode is higher than `OutputInterface::VERBOSITY_VERBOSE`:

```php
dump($this->isVerbose());
```

> You can freely use `dump` method in console commands.

To render table:

```php
$table = $this->table([
    'Column #1:',
    'Column #2:',
]);

foreach ($data as $row)
{
    $table->addRow([
        $row[1],
        $row[2]
    ])
}

$table->render();
```

To add table separator:

```php
$table->addRow(new Symfony\Component\Console\Helper\TableSeparator());
```
