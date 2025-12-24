---
title: Analog to Digital Conversion (ADC)
draft: false
tags:
  - electronics
  - information
  - signals
---
*[[Confidence Indicator|Confidence]]: 2*

# Analog
What is meant by analog? Usually, people are referring to signals you would find in the real world. These are continuous time or frequency signals. This means if you were to plot the signal on a graph, you would see no break in the function.
![[analog_plot_desmos.png]]
*A simple plot of a sinusoid in [Desmos](https://www.desmos.com/). Real world data is presumed to be continuous as shown here.[^1]*
# Digital
What is meant by digital? These are signals that are not only [discrete in time](https://en.wikipedia.org/wiki/Discrete_time_and_continuous_time), but also have discrete values. The simplest case is a signal with two states, 0 or 1, LOW or HIGH. This simple signal can be represented by a [[Units#Bit|bit]]. Often, to make the plot of digital values easier to read and understand, a continuous series of step functions are used. However, the raw data is better represented by a series of Dirac delta functions.
![[digital_plot_desmos.png]]
*A simple plot of a square function in [Desmos](https://www.desmos.com/). The signal is oscillating between the 0 and 1 state.*
# Analog to Digital Conversion (ADC)
At first, converting between the two seems impossible. How do we take something with seemingly *infinite* information in the continuous domain and replicate it in the discrete domain? Fortunately, the [Nyquist Sampling Theorem](https://en.wikipedia.org/wiki/Nyquist%E2%80%93Shannon_sampling_theorem) demonstrates that, assuming your signal has a limit to its highest frequency component, even sparse discrete signals can perfectly capture a continuous wave.

Thus, the method of analog to digital conversion is simple. Have some method to measure a signal periodically, ideally sampling as quickly as possible, and record the results. 

## Voltage ADC
Though, in principle, a person could spend all day measuring a signal periodically and recording the results, two problems arise. First, the person is likely to make mistakes. Second, for high speed signals, the person is too slow. Plus, who wants to sit all day recording measurements? The solution is to use computers to take the measurements for us at up to billions of times a second. If you want a computer to interact with a signal, the signal must be a voltage. This is why I'm focusing on converting a continuous voltage to a discrete voltage; it will likely be the most common ADC.

### Comparator ADC
This is perhaps the most basic and intelligible voltage ADC. You take a series of voltage references and a series of comparators all connected to the voltage under test, $V_{in}$, and each connected to their own voltage reference. The outputs of these comparators are combined into a multiplexer.
![[comparator_ADC.png]]
*Example of a 2-bit ADC Circuit from [Electronics Tutorials](https://www.electronics-tutorials.ws/combination/analogue-to-digital-converter.html).*

A comparator is an electronic device that compares the voltage at two inputs. If the voltage in the $V_{in}$ port is greater than the voltage in $V_{ref}$, then the output goes HIGH. If $V_{in} < V_{ref}$, then the output goes LOW. This makes a comparator a device well suited for converting analog to digital, as it has two analog inputs and one digital output. 

The trouble of a comparator is poor scaling and voltage reference preparation. By poor scaling, I mean to go from 2 to 4 bits of ADC capability, you go from $2^2 -1$ to $2^4-1$ comparators, or from 3 to 15 comparators. Imagine trying to make an 8 bit ADC with 255 comparators! And even if you did pay the price both in component cost and board space cost, you would still struggle with precision. This is because making even a single voltage reference is difficult, let alone hundreds. In the example above, multiple voltage references are made with resistors. In the ideal world, this would work. However, resistance can change due to temperature, mechanical strain, and just time wearing away on the resistor and its electrical contacts. And this is just considering imperfections of the resistor, not electromagnetic interference, imperfections in $V_{ref}$, etc. At some point, the extra bits of ADC become pointless because precision isn't limited by the number of bits representing the voltage signal but instead the noise introduced by the voltage references.

[^1]: Ironically, Desmos is digital, and so this isn't a true continuous signal. Instead, Desmos is just sampling enough points and *interpolating* between points to make the final image look continuous. If you want a true continuous signal on a computer, you need to describe it mathematically, such as using [vector graphics](https://en.wikipedia.org/wiki/Vector_graphics).