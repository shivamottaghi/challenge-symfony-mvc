# challenge-symfony-mvc

## The Mountain
We have reached the Mountain in this BeCode course, finally!

This assignment is more about learning how to work with symfony, its MVC layout ,
the routing component and the basic of twig!

### To work with it, need to install it first

As usual, when trying to work with a new technology, installing comes first. I am on Windows so to install
symfony I had to install scoop on powershell , install symfony , and finally add the path to my environment
variables in my system to be able to access the symfony commands in git bash.

- In powershell run :
  ```shell
    iwr -useb get.scoop.sh | iex
  ```
  - In case of an error  regarding the execution policy after you entered the command line, 
  enter the command below in PowerShell and then try installing symfony again:
  ```shell
    Set-ExecutionPolicy RemoteSigned -scope CurrentUser
    ```

- Then run the command below to install symfony cli:
  ```shell
    scoop install symfony-cli
    ```
- To check to see if you have all the requirements to work with symfony run :
  ```shell
    symfony check:requirements
    ```


### Create a new project in symfony

I started reading through [symfony documentation](https://symfony.com/doc/current/page_creation.html).

I am not going to write every single detail about symfony. Since the documentation is pretty great. Just going 
to include some important commands for my future self who might come back to this exercise.

- To start a web app in symfony :
  ```shell
    # challenge-symfony-mvc is the project's name
    symfony new challenge-symfony-mvc --webapp
  ```
  
### Symfony local web server
You can run symfony with any web server like apache. But it also provides its own web server.
- To start the server run the command below in your app directory:
  ```shell
    symfony server:start
  ```

### Routs
To associate a controller with a public url , you can either go to the config directory and make a yaml file,
or use annotation routs. If your php version is 8 or above you don't need to install anything.

In the documentation's example, the goal is to generate a random number (lucky number) and display it. 
That's all.

In my case I first tried to do it via making a routs.yaml file in config directory, then deleted them all and 
tried to do it with annotation.
There is one more thing to do befo

```php
class LuckyController
{

    #[Route('/lucky/number')]
    public function number(): Response{
        $number = random_int(0 , 100);

        return new Response(
            '<html><body>Lucky number: '.$number.'</body></html>'
        );
    }
}
```
### twig, how to render a template?
Twig is modern template engine for php. If you don't have the twig folder in your vendor directory you need to
install it first. In my case I already had it installed.

To make your controller be able to render a template you need to make your class extends from symfony's base 
AbstractController, by doing that you may have access to its properties and functions, including the render
functionality.

I also made a twig file in the template directory and added the html structure into it, this would be the view
in our mvc structure. 

The render function passes an array of variables to the view, as I've already seen in mvc projects, so it is pretty
logical for me. 

To display php variables in twig we need to use `{{variableName}}`.

Here the lucky number generator example comes to an end. This is how my controller looked like in the end:

```php
namespace App\Controller;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;


class LuckyController extends AbstractController
{

    #[Route('/lucky/number')]
    public function number(): Response{
        $number = random_int(0 , 100);

        return $this->render('lucky/number.html.twig' , [
            'number' => $number
        ]);
    }
}
```