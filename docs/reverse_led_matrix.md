# RPI-RGB-LED-MATRIX

## Concepts
- PWM: pulse width modulation. Any method of representing a signal as rectangular wave.
useful for controlling the average power or amplitude delivered by an electrical signal.
Goal is to control a load; but must be selected carefully in order to smoothly do so. Technique
for controlling the power to a device rapidly switching a signal on and off.
- LSB: least significar bit. In binary representation, the LSB is the digit with the least
weight. It contributes the smallet amout to the overal value.
- Temporal Dithering: Technique where on-time of the PWM signal is rapily switched between
slighly different values instead of maintaining a constant on-time for specific brightness
level. By setting pwm_dither_bits to a value greater than zero, you essentially instruct 
the system to use the lower pwm_dither_bits of the on-time value for dithering. 
Temporal dithering can introduce a slight flickering or grainy appearance to the 
LEDs, especially at lower brightness levels. This is because the human eye can perceive 
the rapid switching between on-time values.


## led-matrix.cc

### RGBMatrix (class):
- **Options(struct)**:
    - **methods**
        - Options: void
        - Validate: bool -> Check if the options are correctly set.
    - **attributes**:
        - **hardware_mapping**: char * -> name of the hardare mapping. "regular" or "adafruit-hat"
        - **rows**: int
        - **cols**: int
        - **chain_length**: int -> number of displays chained together.
        - **parallel**: int -> number of parallel chains.
        - **pwm_bits**: int -> default is 11. but if only dealing with limited comic-colors, 1 is
        just fine. Lower requires less CPU and increases refresh rate. 
        - **pwm_lsb_nanoseconds**: int -> defines precision for setting the on-time of the PWM
        signal. higher value, means smaller adjustements can be made on-time. Reduces
        ghosting effects. Can increase the processing time needed to generate the PWM
        signal, potentially impactinv the frame rate.
        - **pwm_dither_bits**: int -> number of LSBs used for temporal dithering in a PWM signal
        for led control. to achieve a higher refresh rate at the potential cost of
        introducing some visual noise.
        - **birghtness**: int
        - **scan_mode**: int -> 0 = progressive and 1=interlaced. 
            - **0(progressive)**: all rows of the LED display are refreshed sequentially, line
            by line, to create a complete image. Provides smoother and more stabe image for
            the viewer.
            - **1(interlaced)**: only half rows of the LED display are refresed in one pass,
            followed by refreshing the remaining half in the next pass. Can potentially 
            achieve a higher refresh rate compared to progressive scan with the same hardware
            limitations. Can introduce flickering or onterlacing linges, especially for
            moving objects on the display.
        - **row_address_type**: int -> how the row selection is achieved on the LED panel.
            - **0 (direct setting)**: most common. System sends a dedicated row address signal that
            directly correspond to the desired row on the diplay. This signal can be a binary 
            number. 
            - **1 (A/B panels)**: typically used for smaller, more cost-effective LED panels,
            often with a resolution of 64x64 LEDS. Insed of indiviaul row address signals,
            these panels might have two data lines named A and B. By selecting activating
            these lines in combination
        - **multiplexing**: int -> Technique for controlling a large number of LEDs using a fewer
        number of control pins on a microcontroller or driver chip. Achieved by rapidly switching
        the activation of different rows or columns of LEDs, creating the illusion of a 
        constantly lit display by leveraging the persistence of human vision.
            - **multiplexing = 0 (direct)**: no multiplexingno multiplexing.
            - multiplexing = 1 (stripe): multiplexing called row or column stripe multiplexing.
            Leads are grouped into hotizontal or vertical vertices. The control system activates
            stripe at a time, during the desiged LEDs within that strupe and keeping the other off.
            - **multiplexing = 2 (checker)**: represent matrix or checkerboard multiplexing.
            LEDs are divided into a grid, and the control system activates indiviual LEDs by
            selecting a specific row or column combination. This metjod offers the most 
            efficinet use of control pint but requires more complex control compared to stripe
            multiplexing.
        - **disable_harware_pulsing**: bool -> Allows to bypass the built in pulse width modulation
        for controlling the LED display and instead generate the control pulses manually.
        - **show_refresh_rate**: bool -> show refresh rate on terminal.
        - **inverse_colors**: bool
        - **led_rgb_sequence**: const char * -> in case the internal sequece of mapping is not
        RGB, this contains the real mapping. Some panels mix these colors. String of length 
        three which has to contain all characters R, G and B.
        - **pixel_mapper_config**: const char * -> string that describes a sequence of pixel 
        mappers that should be applied to this matrix. A semicolon-separated list of pixel-mappers
        with optional parameter.
        - **panel_type**: const char * -> typically empty string, but some panels need a 
        particular inizialization sequence, so this is used for that.
        - **limit_refresh_rate_hz**: int -> limit the refresh rate of LED panel.

- methods:
- attributes:

