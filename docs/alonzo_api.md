## Pin description
### How to read it?
If you're new to embedded development, the first step is to understand the pin description. The picure below is the pin description of Alonzo board.
The pin description indicates the connection of the pins of the MCU.
<center>
<img src="../img/pins_description_20201231_1250_875.png" alt="pin description" width="900" />
</center>


## GPIO
GPIO stands for *General Purpose Input/Output*. GPIOs have no predefined purpose and are unused by default. It can be used as an input or output, or both, and is controllable by the user at runtime.

```scheme
(gpio-set! gpio-name value)
(device-configure! gpio-name)
(gpio-toggle! gpio-name)
  ```
### Pin to GPIO name in schematic

<center>
<img src="../img/gpio_pin.png" alt="GPIO pin description" width="600" />
</center>

### The GPIO name
A **gpio-name** should be a **symbol**.

The naming rule is *P + port\_rank + port\_number*. **P** implies GPIO.

For an instance, there're 3 predefined LEDs on Alonzo board.
The **LED0** was connected to **PC15**, which stands for *GPIO\_C 15*.

<center>
<img src="../img/led_pin_schematic.png" alt="LED pins description" width="200" />
</center>

You may think such a naming rule looks strange. It's defined by the MCU vendor, so we just follow it.

Fortunately, we don't have to remember the port-name of the LED pins. We can control the LED like this:
```scheme
(define  (loop)
  (usleep 200000)
  (gpio-toggle! 'dev_led0)
  (loop))

(loop)
```

Here's the list of all GPIO port name in Animula:
```scheme
'dev_gpio_pa2
'dev_gpio_pa3
'dev_gpio_pa8
'dev_gpio_pa9
'dev_gpio_pa10

'dev_gpio_pb3
'dev_gpio_pb4
'dev_gpio_pb5
'dev_gpio_pb6
'dev_gpio_pb7
'dev_gpio_pb8
'dev_gpio_pb9
'dev_gpio_pb10
'dev_gpio_pb12
'dev_gpio_pb13
'dev_gpio_pb14
'dev_gpio_pb15
```
You may notice that we don't provide all pins as GPIO ports, because other pins are used for certain peripherals. The list above contains all GPIO ports that you can use in Alonzo board..

### GPIO configure

Before you use a GPIO port, you must configure it, once is enough.
```scheme
(device-configure! gpio-name)
```
- return: unspecified
- gpio-name: symbol

*NOTE: For other predefined port, for example, LED pin, you don't have to configure before using it, since the Animula firmware do it for you.*
*NOTE: Reconfigure the predefined port will cause unexpected activity that you may need to reset the device to make it work. Keep this in mind.*

### GPIO set and toggle
```scheme
(device-set! gpio-name value)
```
- return: unspecified
- gpio-name: symbol
- value: boolean

```scheme
(device-toggle! gpio-name)
```
- return: unspecified
- gpio-name: symbol

### GPIO example

Guess: what does this program do?

```scheme
(define (loop x)
   (usleep 200000)
   (gpio-toggle! 'dev_gpio_pb3)
   (gpio-toggle! 'dev_led0)
   (if (<= x 0)
       0
       (loop (- x 1))))

(display "--------------start-----------\n")
(device-configure! 'dev_gpio_pb3)
(loop 10)
```

## I²C
I²C or I2C, pronounced I-squared-C, is a synchronous, multi-master, multi-slave, packet switched, single-ended, serial communication bus. It is widely used for attaching lower-speed peripheral ICs to processors and microcontrollers in short-distance, intra-board communication.

Well, yes, it's slow, but still more convenient than you communicate with GPIO. Besides, it's a communication bus, which means you can connect multiple devices (the slaves) to a I2C controller (the master). The multiplex communication will be arbitrated by the I2C hardware, so you don't bother to do it in softwaxre.

Now that it's serial communication, why not just use serial port? Because serial ports are asynchronous (no clock data is transmitted), devices using them must agree ahead of time on a data rate.

You may realize that I2C requires 2 pins for serial communication, the data pin (**SDA**), and the clock pin (**SCL**), here's the typical I2C connection description:

<center>
<img src="../img/i2c.png" alt="I2C pins description" width="300" />
</center>

*Exercise: try to find out all I2C pins in **Pins Description** and **Pins Schematic**.*

You may notice that **I2C2_SDA** is **PB3**, yes, the second I2C pin was occupied by **GPIOB_3**. So you may not use **PB3** as a GPIO. But if you really want, you can reconfigure it, then it becomes a GPIO, till you reset the device. Unless you're a veteran, you don't want to do that, but it's useful to learn the theory.

### I2C device name
We provide these I2C devices in Alonzo board:
```scheme
'dev_i2c2
'dev_i2c3
```
<!--  -->
### I2C APIs
```scheme
(i2c-read-byte! dev_name i2c_address i2c_register)
```
- return: integer in byte
- dev_name: symbol
- i2c_address: integer in byte
- i2c_register: integer in byte

```scheme
(i2c-write-byte! dev_name i2c_address i2c_register value)
```
- return: integer in byte
- dev_name: symbol
- i2c_address: integer in byte
- i2c_register: integer in byte
- value: integer in byte

OK, so what are I2C addresses and registers? Where to find them? Are they defined by Animula?

No, they're not defined by Animula, you can not find them in Animula.

The answer is on the datasheet of your I2C peripheral. For example, you have a compass censor with I2C interface. You should find its I2C address and registers definition on its datasheet. Let me show you an example, it's from **HMC5883L Compass datasheet**:

<center>
<img src="../img/HMC5883L_Compass_reg.png" alt="HMC5883L Compass registers" width="800" />
</center>

### I2C example
```scheme
(define (convert-to-int16 a b)
  (let ((c (+ (* 256 a) b)))
    (if (> c 32767)
        (- c 65536)
        c)))

(define (mma8451_connection_check!)
  (let* ((MMA8451_DEFAULT_ADDRESS 29) ;; 0x1D
         (MMA8451_REG_WHOAMI 13) ;; 0x0D
         (MMA8451_REG_CTRL_REG1 42) ;; 0x2A
         (ret     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS MMA8451_REG_WHOAMI))
         )
    (if ret
        (if (= ret 26)
            #t
            #f)
        #f)))

(define (mma8451_set_state_active!)
  (let* ((MMA8451_DEFAULT_ADDRESS 29) ;; 0x1D
         (MMA8451_REG_WHOAMI 13) ;; 0x0D
         (MMA8451_REG_CTRL_REG1 42)) ;; 0x2A
    (i2c-write-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS MMA8451_REG_CTRL_REG1 1)
    (let  ((ret     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS MMA8451_REG_CTRL_REG1)))
      (display "ret = ")
      (display ret)
      (newline)
      (if ret
          (if (= ret 1)
              #t
              #f)
          #f))))


(define (mma8451_read_all!)
  (let* ((MMA8451_DEFAULT_ADDRESS 29) ;; 0x1D
         (MMA8451_REG_WHOAMI 13) ;; 0x0D
         (MMA8451_REG_CTRL_REG1 42) ;; 0x2A
         ;; (reg0     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS 0))
         (reg1     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS 1))
         (reg2     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS 2))
         (reg3     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS 3))
         (reg4     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS 4))
         (reg5     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS 5))
         (reg6     (i2c-read-byte! 'dev_i2c2 MMA8451_DEFAULT_ADDRESS 6))
         )
    (display "x = ")
    (display (convert-to-int16 reg1 reg2))
    (display ", y = ")
    (display (convert-to-int16 reg3 reg4))
    (display ", z = ")
    (display (convert-to-int16 reg5 reg6))
    (display "\n")))

(define (loop x)
  (usleep 333333)
  (mma8451_read_all!)
  (if (<= x 0)
      0
      (loop (- x 1))))

(define (main)
  (ble-enable!)
  (if (mma8451_connection_check!)
      (begin (display "MMA8451Q connection OK!\n")
             (mma8451_set_state_active!)
             (loop 90))
      (display "Connection FAIL!\n"))
  (newline))

(main)
```
You will see the result:

<img src="../img/i2c-test.jpeg" title="I2C test" alt="I2C test"/>
