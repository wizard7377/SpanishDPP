\page sencillo-cpp Tu primero C++ bote

Después de tu obtuviste un autentificado de tu bote, y pusiste tu bote un tu servidor, es probablemente tu quires escribir algo C++, ¡asi que nosotros escribimos algo ahora! *No te preocupas si tu no comprendas todo la información*, esto es tu primero tiempo con D++, y otros guías en la documentación va a mas detalles.

\note La pagina no enseña tu cómo compilar un proyecto con D++, consulta la pagina apropiada para tu y tus instrumentales

1. Crea un fichero te llama `main.cpp` y incluya D++
~~~{.cpp}
#include <dpp/dpp.h>
~~~
2. Pon tu autentificado tu obtienes [antes de hoy](\ref guía-primero) en una variable por el futuro (copia y pega lo en la cadena de caracteres)
~~~{.cpp}
const std::string BOT_TOKEN = "";
~~~
3. Crea un función `main` (Después de hoy, di por sentado todo lo código es en `main`)
~~~{.cpp}
int main() {
}
~~~
4. Crea un `dpp::cluster` objecto con la variable de antes. `dpp::cluster` es la clase más importante en D++, el es como tu recibes eventos de Discord.
~~~{.cpp}
dpp::cluster bot(BOT_TOKEN);
~~~
5. Configurar D++ a salida a salida norma.
~~~{.cpp}
bot.on_log(dpp::utility::cout_logger());
~~~
6. Es una etapa **muy** importante, asi que **¡prestar atención!**. Primero, mira al `bot.on_slashcommand([](const dpp::slashcommand_t& event)`. `dpp::cluster` se encargues todos los comandos de Discord, y tu interactúas con los con callbacks, usualmente en la forma de lambdas. En la lambda (`[](const dpp::slashcommand_t& event)`), nosotros vimos que la lambda registra nunca del externo (`[]`), y tiene un parámetro de `const dpp::slashcommand_t&` nos llama `event`. `dpp::slashcommand_t` tiene *toda* la información del evento, y todos los eventos de Discord son usa un callback con un parámetro es parecerse a `const dpp::*_t&`, por ejemplo, esto `const dpp::slashcommand_t&`, o `const dpp::button_click_t&` (entre otros).
~~~{.cpp}
bot.on_slashcommand([](const dpp::slashcommand_t& event) {

});
~~~
7. Pon el código en tu lambda. `command` es un atributo de `event` de tipo `dpp::interaction`, que mas o menos representa los datos del evento. Aquí, nosotros usamos su método `get_command_name()`, que obtiene el nombre de la comandos, en este caso, nosotros chequeo si la entrada usuario fuera `/ping`, y en caso de que es `/ping`, usarías el método `reply` de `event`, que responde con un la cadena de texto al mensaje.
\note Discord va a devolver un error a tu usuario si no respondieras **exacto** un tiempo, no mas o menos. 
~~~{.cpp}
bot.on_slashcommand([](const dpp::slashcommand_t& event) {
    if (event.command.get_command_name() == "aprendiendo") {
        event.reply("D++!");
    }
});
~~~
8. `on_ready` es similar a `on_slashcommand`, el registra lambdas por eventos. Especialmente, `on_ready` llama la lambda cuando D++ se conecta a Discord. `dpp::run_once<struct register_bot_commands()` es simplemente en caso que D++ conecta y desconecta a Discord, asi que D++ no registra tus comandos dos veces. Finalmente, el parte de la código que actualmente crea los comandos, `dpp::slashcommand()` crea un comando vació, y su nombre configura a `aprendiendo` con `set_name("aprendiendo")`, y su descripción configura con `set_description("algo")`, ambos devolve el `dpp::slashcommand` objecto, asi que ellos pueden ligan.
~~~{.cpp}
bot.on_ready([&bot](const dpp::ready_t& event) {
    if (dpp::run_once<struct register_bot_commands()) {
        bot.global_command_create(
            dpp::slashcommand().set_name("aprendiendo").set_description("algo")
        );
    }
});
~~~
9. Finalmente, nosotros necesitamos encender el bote, el razón que usamos `dpp::st_wait` es complejo, y yo no cubre lo aquí.
~~~{.cpp}
bot.start(dpp::st_wait);
~~~
10. Si tu seguías las etapas en el tutorial, tu código debe hoy mas o menos similar a este:
~~~{.cpp}
#include <dpp/dpp.h>
 
const std::string BOT_TOKEN = "TU AUTENTIFICADOR AQUÍ";
 
int main() {
    dpp::cluster bot(BOT_TOKEN);
 
    bot.on_log(dpp::utility::cout_logger());
 
    bot.on_slashcommand([](const dpp::slashcommand_t& event) {
        if (event.command.get_command_name() == "aprendiendo") {
            event.reply("D++!");
        }
    });
 
    bot.on_ready([&bot](const dpp::ready_t& event) {
        if (dpp::run_once<struct register_bot_commands>()) {
            bot.global_command_create(
                dpp::slashcommand().set_name("aprendiendo").set_description("algo")
            );
        }
    });

    bot.start(dpp::st_wait);
}
~~~
11. ¡Felicidades! ¡tu creas tu primero Discord bote con D++!
