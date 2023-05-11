# apuntes

Este proyecto es solo un apunte para cubrir los aspecttos
de lo que al lenguaje MOTOKO se refiere

MOTOKO es un lenguaje precompilado que traduce el códgo
a WA webassembly. De esa manera permite mejor interoperabilidad
entre las distintas arquitecturas existentes, sin la necesidad
de recompilar el software dpeendiendo de si será abierto en
linux, en un x86 o x64 u otros.

Trabajo con los llamados cannisters, que ya tienen incorporados
dentro de sí mismos los módulos de web assembly

Los cannisters operan como nodos que deciden cuándo interactuar
otros, si necesitan crear otros cannisters, si necesitan destruirse
o destruir otros. Qué hacer la próxima vez que alguien interactúe con ellos.


Para lograr esto tenemos a los actores, que se encargan
de la comunicación de mensajes. Es una pequeña pieza de código
dentro del cannister y que va dentro del main.mo del backend

```
actor {

    /// CODE

};
```

### ACTORS

Un actor es la representación abstracta del cannister:

Have a private state (memory) & can run computations.
Receive messages from users or other canisters.
Can send messages to users or other canisters.
Can create other canisters.


```
actor {
    var message : Text = "Hello Motoko Bootcamp!";

    public func changeMessage(t : Text) : async () {
        message := t;
    };

    public query func readMessage() : async Text {
        return message;
    };
};
```

En el código de más arriba podemos ver la declaración e inicialización de variables
para el String tenemos Text
para las funciones tenemos el encapsulamiento de public
en un canister el public indica la posibilidad de otros canisters
de poder interactuar con esta función que está expuesta, así como
con el usuario


```
 public func changeMessage(t : Text) : async () {
        message := t;
    };
```

en este fragmento del código nosotros creamos una función
que recibirá un argumento 't' de tipo 'Text'
la función es asíncrona denotada por 'async()
y en el cuerpo de la misma, a la espera de t
nosotros tras recibir t asigmanos este a message

```
  public query func readMessage() : async Text {
        return message;
    };
```

la palabra reservada query en la función indica que la misma solo será de tipo
lectura, y retornará un tipo de dato indicado en el ASYNC sin paréntesis

### CANISTERS Y SU BATERÍA

los canisters tienen una vida util determinada por la batería, sus ciclos.
los ciclos que le colocas se queman a medida que performan interacciones.

1 XDR se computa como 1 trillon de ciclos
los costos en términos de gas se encuentran en la siguiente página:
https://internetcomputer.org/docs/current/developer-docs/gas-cost


### PROGRAMACIÓN
💻

## VARIABLES
al igual que javascript estas poseen el let y var
let es inmutable una vez declarada junto con su valor

var, por otro lado puede ser mutada sin problema

```
// Las variable se inicializan con el tipo
// y el tipo de dato que contendrán luego del
// uso de los dos puntos ':'

let variable1 : String = "String";
var variable2 : String = "String";

// y al igual que java hace uso estricto del punto y coma
// pueden no declarar el tipo de dato que usarás
// y hará uso de la inferenciación y lo asignará en automático
// según el primer valor asignado.
```

## FUNCIONES
Las funciones pueden existir tanto dentro como fuera de los actors
de momento nos enfocaremos en las que pueden dentro de los actors.

Tienen un encapsulamiento de dos tipos 'public' y 'private'.
Usamos las public para interactuar con otros cannisters, y deben
ser de tipo async. 

Las private las usaremos como soporte para las publics

```
actor {
    var count : Nat = 0;

    // esta función está siendo usada como función auxiliar 
    // de la principal
    private func add(n : Nat, m : Nat) : Nat {
        return (n + m)
    };

    // El uso de async es por la espera de 'n' que recibiremos como argumento
    // pero debido a que habrá un delay siempre indicamos con asyn que debemos esperar
    // el Nat a la derecha de async es lo que devolveremos asincrónicamente
    public func addCount(n : Nat) : async Nat {

        // hacemos uso de la función add()
        let newCount = add(count,n);
        count := newCount;

        // el uso de return es opcional, de hecho,
        // podríamos solo poner count y con eso ya sería un return
        return count;
    };
}```


## TIPOS DE DATOS

Tenemos varios, al rededor de 8-9

Bool, Nat (que son los Int sin firmar), Int, Float32 (que son de precision simple), Float64 (que son de doble precisión),
Text (es el String de motoko), Char.

```
  let isTrue : Bool = True;

  let number : Int = 42;
  let notSignedNumber : Nat 10;

  let float32Number : Float32 = 3.14;
  let float64Number : Float64 = 2.71828;

  let greeting: Text = "Hello, World!";
  // este greeting # es una variable concatenada a un string " Welcome!"
  let concatenated: Text = greeting # " Welcome!";

  let firstChar: Char = "A";
  let secondChar: Char = "B";
```

## ESTRUCTURAS DE DATOS
En estas tenemos las listas y los arrays

Lists, son dinámicas y permiten una concatenación y manipulación
bastante eficiente.

Arrays, por otro lado, al igual que en java son de tamaño
fijo y para redefinir su tamaño debes crear uno nuevo
con el nuevo tamaño y luego reasignar este array
a la variable que almacenaba el viejo array.

```
  let myList: [Int] = [1, 2, 3, 4, 5];
  let myArray: Array<Int> = Array{5}(0);
```


















##### below this is the automatic text generated
#### when you create a dfx new projectname



Welcome to your new apuntes project and to the internet computer development community. By default, creating a new project adds this README and some template files to your project directory. You can edit these template files to customize your project and to include your own code to speed up the development cycle.

To get started, you might want to explore the project directory structure and the default configuration file. Working with this project in your development environment will not affect any production deployment or identity tokens.

To learn more before you start working with apuntes, see the following documentation available online:

- [Quick Start](https://internetcomputer.org/docs/current/developer-docs/quickstart/hello10mins)
- [SDK Developer Tools](https://internetcomputer.org/docs/current/developer-docs/build/install-upgrade-remove)
- [Motoko Programming Language Guide](https://internetcomputer.org/docs/current/developer-docs/build/cdks/motoko-dfinity/motoko/)
- [Motoko Language Quick Reference](https://internetcomputer.org/docs/current/references/motoko-ref/)
- [JavaScript API Reference](https://erxue-5aaaa-aaaab-qaagq-cai.raw.icp0.io)

If you want to start working on your project right away, you might want to try the following commands:

```bash
cd apuntes/
dfx help
dfx canister --help
```

## Running the project locally

If you want to test your project locally, you can use the following commands:

```bash
# Starts the replica, running in the background
dfx start --background

# Deploys your canisters to the replica and generates your candid interface
dfx deploy
```

Once the job completes, your application will be available at `http://localhost:4943?canisterId={asset_canister_id}`.

If you have made changes to your backend canister, you can generate a new candid interface with

```bash
npm run generate
```

at any time. This is recommended before starting the frontend development server, and will be run automatically any time you run `dfx deploy`.

If you are making frontend changes, you can start a development server with

```bash
npm start
```

Which will start a server at `http://localhost:8080`, proxying API requests to the replica at port 4943.

### Note on frontend environment variables

If you are hosting frontend code somewhere without using DFX, you may need to make one of the following adjustments to ensure your project does not fetch the root key in production:

- set`DFX_NETWORK` to `ic` if you are using Webpack
- use your own preferred method to replace `process.env.DFX_NETWORK` in the autogenerated declarations
  - Setting `canisters -> {asset_canister_id} -> declarations -> env_override to a string` in `dfx.json` will replace `process.env.DFX_NETWORK` with the string in the autogenerated declarations
- Write your own `createActor` constructor
