# *Setup* Básico Con Digilent *Nexys Video* #
## Síntesis de RTL con Vivado ##
* Crear un "Block Design" de Vivado.![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_022002.png "Creando un Diseño de Bloques") ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_022833.png "Agregar IP")
* Importar *Constraints*, si se desea agregar RTL (Prefiera activar copiar el .xdc dentro del proyecto).	
* Agregar un "Microblaze" (CPU principal). ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_022848.png "Agregando un CPU Microblaze")
* Configurar el CPU principal
   * Ejecutar "Run block Automation". Esto configurara la memoria DRAM y el reloj. ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_023459.png "Agregando un CPU Microblaze")
   * Especificar el tipo de CPU que se desea sintetizar. Para linux, se recomienda *Application*. También puede especificarse memoria local. Podría ser necesaria en caso de necesitar maniobrar con el bootloader antes de que la memoria DRAM se inicialice. Como no hay reloj, especificar *New clocking Wizard*. ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_023550.png "Configurado Microblaze, Confirmar con Ok")
Cerrar la ventana con <kbd>OK</kbd>. Esto tardará algunos instantes mientras agrega los bloques de interconexión necesarios.
* Configurar El reloj: Doble clic sobre el el bloque *Clock Wizard*.
   * Ir a la seccion "*Board*".
     * Asegurarse que esté seleccionado `sys_clk` en `CLK_IN1` y `reset` en `EXT_RESET_IN`. ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_025541.png "Configurado *Clock Wizard*")
  * Ir a la seccion "*Output Clocks*".![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_030639.png "Configurado *Clock Wizard*")
    * Habilitar `clk_out2` y darle una frecuencia de 200 Mhz. Esta será la frecuencia de la DRAM.
    * Seleccionar `clk_out3` y darle una frecuencia de 125 Mhz. Esta sería la frecuencia del IP MAC de la Red Ethernet. Habilítela sólo si agrega el IP de Ethernet.
    * Cambiar la polaridad del *Reset* a "Active Low".![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_031051.png "Configurado *Clock Wizard* y Polaridad del *Reset*"). Cerrar la ventana con <kbd>OK</kbd>. Esto tardará algunos instantes.
* Configurar las líneas de Interrupción. El controlador de interrupciones recibe líneas de interrupción como un vector lógico, pero los dispositivos tienen salidas de interrupción de tipo bit, por lo tanto debe usarse un bloque concat para cambiar el tipo: 
  * Hacer doble clic en el bloque concat
  * Cambiar la cantidad de líneas de 2 a las que sean necesarias ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_031207.png "Configurado líneas de interrupción") ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_031216.png "Resultado de Cambiar la cantidad de líneas"). Cerrar la ventana con <kbd>OK</kbd>. Esto tardará algunos instantes.
* Agregar y Configurar IP's
    * Agregar "AXI QUAD SPI" "AXI Timer" "AXI UART16550", "AXI GPIO" y "MIG 7". ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_031216.png "Resultado de Cambiar la cantidad de líneas")
    * Configurar bloque "MIG 7" Seleccionando "Run Block Automation" ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_031802.png "Configurando MIG"). Cerrar la ventana con <kbd>OK</kbd>. Esto tardará algunos instantes. ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_031823.png "Configurando MIG"). Se produce un error relacionado con las interfaces de la tarjeta. Este error no tiene mayores consecuencias y puede ser ignorado con seguridad.
    * Seleccionar en el Pop-up "Run Connection automation". Esto conectará el bus de sistema de los IP's agregados.![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_032203.png "Configurando Autoconexión"). Se deben seleccionar todos menos `microblaze_0` (Ya se ha coenctado). Cerrar la ventana con <kbd>OK</kbd>.
    * Cambiar el reloj de la memoria DRAM al `clk_out2`. Observe que el reloj del controlador de memoria es el mismo que el de el bus del sistema. Si se deja en estas condiciones, se estará desaprovechando la velocidad de la memoria ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_032915.png "Red de Reloj")
      * Desconectar el pin `sys_clk_i` (Botón derecho, "Disconnect Pin") ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_032915.png "Desconexión `sys_clk_i`")
      * Conectar `sys_clk_i` con `clk_out2` ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_033049.png "Conexión `sys_clk_i` con `clk_out2`").
     * Conectar la memoria DRAM al controlador seleccionando el pin `DDR3`, botón derecho y seleccionando "Make External" ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_034008.png "Conexión `DDR3`")![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_034027.png "Conexión `DDR3` lista")
    * Habilitar las interrupciones del bloque "AXI GPIO" ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_035404.png "Habilitando Interrupciones")
    * De la pestaña board, conectar "QSPI Flash", "5 Pushbuttons" y "8 leds"
    * Conectar al bloque concat las interrupciones de los IP's que se agregaron ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-petalinux-doc/Screenshot_20200421_040324.png "Conectando Interrupciones")
        
    
### Opcionales ###
* Agregar BRAM 
  * Agregar el bloque *BRAM Controller*.
  * Agregar el bloque *Block Memory Generator*.
  Pueden configurarse las caracteristicas de la BRAM, como la cantidad de puertos, el ancho y la capacidad máxima.

* Crear *Wrapper* de HDL(botón derecho sobre el diseño de bloques (el archivo .bd, en la pestaña *Sources*). (Esto encapsula el diseño y lo hace referenciable por el simulador y el sintetizador).

![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_193758.png "Configurando GPIO")
* Generar bitstream (Esto podria tardar hasta una hora)
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_194026.png "Configurando GPIO")
* Exportar Hardware, incluyendo el bitstream
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200420_023711.png "Configurando GPIO")

## Aplicaciones *Baremetal* Con Xilinx Vitis ##

Después de exportar el hardware, Abrir Xilinx Vitis. Al ser similar a Eclipse creará un *Workspace*. Los archivos de código fuente y del proyecto quedarán guardados en este entorno protegido, mientras se desarrolla la aplicación.

* Crear una aplicación *Baremetal* 
 1 Ir a "File"->"Project"->"Application"
 2 Darle un nombre al proyecto. No ponerle espacios al nombre.
 * Selecionar la plataforma del proyecto. Puede seleccionarse una plataforma existente (Lamentablemente está *buggy* no ocupar) o crear una nueva. ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-vitis-doc/Screenshot_20200423_183739.png "Configurando proyecto de Aplicación")
  * Al presionar <kbd>Next</kbd>, debe seleccionarse que método será usado para generar la plataforma. ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-vitis-doc/Screenshot_20200423_183750.png "Configurando Plataforma"). Seleccionar "Create new platform from Hardware (XSA)"
  * La ventana tiene un campo donde se listan plataformas recientes disponibles en los ejemplos o bien dentro del *Workspace*. Seleccionar "Create new platform from Hardware (XSA)".Si se necesita importar una plataforma de elaboración propia (el presente caso), debe hacer clic en "+" y seleccionar la carpeta donde se encuentra el archivo `.xsa` que se generó al exportar hardware con Vivado. ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-vitis-doc/Screenshot_20200423_183803.png "Importando Hardware")
   * Al presionar <kbd>Next</kbd>, debe seleccionarse el *Domain* de la plataforma. En este caso, en cada campo debe seleccionarse "Microblaze_0"(Podría haber más de un procesador independiente), "Standalone" y "C". ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-vitis-doc/Screenshot_20200423_183813.png "Seleccionando Dominio")
   * Al presionar <kbd>Next</kbd>, debe seleccionarse un *Template* para este caso, se seleccionará "Hello World".
   * Al presionar <kbd>Next</kbd> quedará disponible el entorno de vitis
   ![TEXTO_DESC](https://github.com/ColdfireMC/nexysvideo-microblaze-petalinux-demo/blob/master/microblaze-vitis-doc/Screenshot_20200423_183915.png "Estado inicial del proyecto")
   
  
 
 
 
 
 
 
### Aplicaciones *Baremetal* Con Xilinx Vitis ###
