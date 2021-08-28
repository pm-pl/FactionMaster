# FactionMaster Plugin
###### FactionMaster, the faction plugin extensible and modulable to your way
## Extension base
Exemple : [here](https://github.com/ShockedPlot7560/FactionMaster-Extension-Example)

Your main class must implements [ShockedPlot7560\FactionMaster\Extension\Extension](https://github.com/ShockedPlot7560/FactionMaster/blob/master/src/ShockedPlot7560/FactionMaster/Extension/Extension.php)
The execute() function will be called when the extension is loaded, so please put all the following content in it.

**The registerExtension of [ShockedPlot7560\FactionMaster\Extension\ExtensionManager](https://github.com/ShockedPlot7560/FactionMaster/blob/master/src/ShockedPlot7560/FactionMaster/Extension/ExtensionManager.php) must be in the onLoad() function**

### Official extension list
* [Invitation improve system](https://github.com/ShockedPlot7560/FactionMaster-InvitationImprove)
* [Admin tools](https://github.com/ShockedPlot7560/FactionMaster-AdminTools)

## Register route
```php
use ShockedPlot7560\FactionMaster\Route\RouterFactory;
use ShockedPlot7560\FactionMaster\Route\Route;

RouterFactory::registerRoute(new CustomRoute());

class CustomRoute implements Route {
    //TODO: implements function
    // __invoke : function to call when the Route are requested
}
```
## Call and send Route
```php
use ShockedPlot7560\FactionMaster\Utils\Utils;
use ShockedPlot7560\FactionMaster\Route\RouterFactory;

Utils::processMenu(RouterFactory::get($SLUG), $PmmpPlayer, [$params]);
```
## Create Button Collection
The collection allows you to manage lists of buttons according to the permissions of each player
```php
use ShockedPlot7560\FactionMaster\Button\Collection\Collection;

class CollectionTest extends Collection {
    public function __construct() {
        parent::__construct($Slug);
        
        // Registration of a function that will be called 
        // to generate the buttons when calling the init() function
        $this->registerCallable($callableName, function(Player $player, UserEntity $user /*more argument if wanted */) {
            // The function has in default panel, the $plyaer and $user instance given
            $this->register(new Button(/*TODO: add argument*/));
        });
    }
}

```

## Modified existing button menus
```php
use ShockedPlot7560\FactionMaster\Button\Collection\CollectionFactory;
use ShockedPlot7560\FactionMaster\Button\Collection\MainCollectionFac;

$ButtonCollection = CollectionFactory::get(MainCollectionFac::SLUG);
$ButtonCollection->registerCallable("FactionMasterExample", function() use ($ButtonCollection) {
  $ButtonCollection->register($newButton, $index, $override);
});
```
## Register new permission
Register new permissions that will be proposed in the Permissions management menu according to the grades
```php
use ShockedPlot7560\FactionMaster\Main;
use ShockedPlot7560\FactionMaster\Permission\Permission;

$PermissionManager = Main::getInstance()->getPermissionManager();
$PermissionManager->registerPermission(new Permission(
    //TODO: put the good argument
), $overrideStatus);
```
## Register new translation SLUG
On Main class of the Extension, the getLangConfig() function, return an array contain the config like :
```php
array(
    "fr_FR" => new Config($this->getDataFolder() . "fr_FR.yml", Config::YAML)
);
```
