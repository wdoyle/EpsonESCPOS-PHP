# EpsonESCPOS-PHP
This project is to allow you to print to your EPSON Thermal Printer via PHP.
This has been tested on Windows XP, Windows 7, Centos and Debian as well as a Raspberry PI

My current setup is a Raspberry PI 2 with a PI Serial adapter from ABElectronics
https://www.abelectronics.co.uk/products/3/Raspberry-Pi/29/Serial-Pi

In terms of software I have had this working on Apache2 on Linux, XAMPP and WAMP on Windows - All other web servers should support this!

This supports all versions of EPSON Serial Printers. During the main part of my testing I was using  TM88III, TM88IV printers.


#Installation and Configuration
    #Windows:
        COM1

    #LINUX:
        /dev/ttyS1

        For linux devices it is best to do: dmesg | grep tty (This will show you your available serial devices)

    #RASPBERRY PI with PISERIAL
        /dev/ttyAMA0

    #Common Issues
        If you have issues with the font be garbage on the page then you will need to change Port Speed

        #Windows
            Open Start -> Computer -> Right Click -> Manage Computer -> Devices -> Ports -> Find your port and click Properties -> Click Configure -> Edit the Settings

            OR

            Open CMD -> Windows Key and R -> CMD -> Enter -> MODE [SERIALPORT]:[PORT SPEED],N,8,1,P
            For reference: http://www.computerhope.com/issues/ch000245.htm


        #LINUX
        You might want to try: stty -F /dev/tty[PORT] [SPEED]  however linux is usually out the box and it works.
        PLEASE NOTE: On Linux you may need to do: chmod a+x /dev/tty[DEVICE] as by default you only have Read Access to the Coms port

#Youtube Video
This is a link to show you what it looks like when printed:
http://www.youtube.com/watch?v=k1KTLC01mtM

#Sample Code

        //For this example I am using my Raspberry PI
        $printer = new phpprint("/dev/ttyAMA0");
        //We put a space between the top of the page and the content
        $printer->newline();
        // We set the font to something that I would define as slightly more sexier
        $printer->set_font(phpprint::FONT_B);
        // We then set the justification to CENTER - This will print everything center aligned
        $printer->set_justification(phpprint::JUSTIFY_CENTER);
        // We now set the font larger for our Text Logo
        $printer->enlargePrint(true);
        // We simply add our text for the logo (Note: The \n will ensure the next piece of content will go to a new line
        $printer->text("FuelledUK EPOS\n");
        // We are changing the font to show you the difference
        $printer->set_font(phpprint::FONT_A);
        // We print some more text to fill the page
        $printer->text("www.fuelled.co\n");
        // Lets add some space before we start placing with Colours
        $printer->newline();
        // We are now telling the printer we want everything to be printed in opposite. So black background and white text
        $printer->reverse_mode(1);
        // Generate Random ID (Test only)
        $rand = rand(200000,999999);
        // As you will see there is slightly bigger space between the text so that it looks good on the print out
        $printer->text("   CUSTOMER ID: ".$rand."    \n");
        // We now reset the printer back to normal printing mode
        $printer->reverse_mode(0);
        $printer->enlargePrint();
        // We then apply 2 lines of space. Feed is best for when you want bigger gaps
        $printer->feed(2);
        // This function will generate you a barcode - Simply specify the content, barcode type and height
        $printer->generateBarcode($rand, phpprint::BARCODE_CODE39, 80);
        // More spacing
        $printer->feed(2);
        // Add some normal text to the page with different font
        $printer->set_font(phpprint::FONT_C);
        $printer->text("NEW CUSTOMER: WARREN DOYLE\n");
        // Always ensure you add space at the end of the print so you can easily collect it from the printer
        $printer->newline();
        // Finally cut the page
        $printer->cut();
        // If you are creating multiple pages, I would ensure you put
        // sleep(1); so that the print doesn't get congested and stop working