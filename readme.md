# *Setup* Básico Con Digilent *Zybo* o *PYNQ* #

* Crear un "Block Design" de Vivado.
* Importar *Constraints*, si se desea agregar RTL (Prefiera activar copiar el .xdc dentro del proyecto.	
* Agregar un PS "ZYNQ 7 Processing System" (CPU principal).
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_181928.png "Agregando un PS")
* Configurar el CPU principal.
    * Ejecutar "Run block Automation". Esto configurara la memoria DRAM y el reloj.
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_182012.png "Autoconfigurar PS")
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_182859.png "Autoconfigurar PS")
Cerrar la ventana con <kbd>OK</kbd>
    * Doble clic al bloque ZYNQ (Esto produce el comando "Re-customize" IP).
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_182859.png "Configurando Interrupciones del PS")
        * Ir a la seccion "*Interrupts*".
        * Habilitar "Enable PS-PL Interrupts".
        * Habilitar "Enable Shared Interrupts".
       
* Conectar `FCLK_CLK0` con `M_AXI_GP0_ACLK`. Este reloj tambien sera el reloj principal del bus AXI.
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_191246.png "Conectar Reloj Principal al Bus AXI"). (Los *Warnings* no generan mayores problemas).
* Agregar y Configurar IP's
    * Agregar *Processor System Reset* y *Run Connection Automation*
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_191335.png "*Run Connection Automation*")
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_192129.png "*Run Connection Automation*")
     ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_192143.png "*Run Connection Automation*")
    
    * Agregar *Concat*, *AXI IIC*, *AXI QUAD SPI* y 2 *AXI GPIO*.
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_192221.png "Agregando Concat")
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_192323.png "Agregando GPIO")
    * Configurar bloque *Concat*: Dado que el tipo de `IRQ_F2P` no es compatible con las lineas de interrupcion, deben concatenarse las lineas en un solo vector lógico. Este bloque puede juntar las lineas de interrupción. Debe agregarse una entrada por cada interrupción
         * Doble Clic en *Concat*
         * Editar el ampo *Number of Ports*. En este caso serian 4 lineas (Una por cada dispositivo con Interrupción).
         ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_192529.png "Configurando Concat")
    * Configurar *AXI GPIO*: Doble clic en el bloque
      * Habilitar Interrupciones
      ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_192347.png "Configurando GPIO")
      * Puede seleccionarse a que conectar cada interfaz GPIO, a las conexiones disponibles en el preset de la placa. Si se desea conectar RTL o usar los nombres de los .xdc, debe seleccionarse *Custom* y crear un puerto (Véase Agregar RTL).
      ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_193423.png "Configurando GPIO")
      ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_193445.png "Configurando GPIO")
    * Los otros bloques usan en este caso la configuración predeterminada.
* Conectar los bloques de IP: Puede autoconectarse los bloques IP AXI. Nótese que la conexión automática no conecta las interrupciones y solo puede autoconectar un reloj.
  * Hacer clic en el pop-up *Run Connection Automation
  * Seleccionar todos los bloques en las casillas.
  Esto hará que se genere automáticamente un bloque de interconexión y arbitraje AXI y las líneas de dirección y datos del bus.
  * Conectar Lineas de Interrupción
    * Conectar La salida del bloque concat al *ZYNQ Processor System*.
    * Conectar cada una de las líneas de interrupción de los bloques IP perifericos agregados al bloque *Concat*.
    ![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_193211.png "Configurando GPIO")
    
        
    
### Opcionales ###
* Agregar BRAM 
  * Agregar el bloque *BRAM Controller*.
  * Agregar el bloque *Block Memory Generator*.
  Pueden configurarse las caracteristicas de la BRAM, como la cantidad de puertos, el ancho y la capacidad máxima.
* Agregar Interfaz de gestion del Ethernet PHY (Sólo Zybo).
  * Descomentar las lineas 49 y 50 
  ``` 
  set_property -dict { PACKAGE_PIN F16   IOSTANDARD LVCMOS33 } [get_ports eth_int_b]; #IO_L6P_T0_35 Sch=ETH_INT_B
  set_property -dict { PACKAGE_PIN E17   IOSTANDARD LVCMOS33 } [get_ports eth_rst_b]; #IO_L3P_T0_DQS_AD1P_35 Sch=ETH_RST_B
  
  ```
  * Crear puertos con los nombres `eth_int_b` y `eth_rst_b`.
  * Conectar `eth_int_b` a concat y `eth_rst_b`, a `peripheral_aresetn`
  
## Generación de Productos ##
* Crear *Wrapper* de HDL(botón derecho sobre el diseño de bloques (el archivo .bd, en la pestaña *Sources*). (Esto encapsula el diseño y lo hace referenciable por el simulador y el sintetizador).

![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_193758.png "Configurando GPIO")
* Generar bitstream (Esto podria tardar hasta una hora)
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200416_194026.png "Configurando GPIO")
* Exportar Hardware, incluyendo el bitstream
![TEXTO_DESC](https://github.com/ColdfireMC/pynq-demo/blob/master/Screenshot_20200420_023711.png "Configurando GPIO")




