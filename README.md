# Signature Pad for SmartForms
## Introduction
With the Signature Pad, you can capture signatures as part of the approval steps of the request. One can scribble using your mouse or finger on touch-screen devices. You can also redraw the signature for readonly views of the request.


## Included Components
The Signature Pad component has two views, one allowing you to capture signatures and one to redraw these in a readonly manner. Both can also reside on a form at the same time. These make use of a cool third-party script to handle the signature action and data interpretation. There are some additional properties that can be set but I didn't see the need for that right now, details here:

[GitHub - Signature Pad](https://github.com/szimek/signature_pad)


## Using the Signature Pad Views in your Forms
### Capturing a Signature
Use the view called *Signature Pad - Sign*, located in the **Common/Signature Pad** category. Simply add the view to your form and initialize it by:
1. Adding (or editing) the *When the server loads the Form* rule
1. Add an action of *Execute a server rule*
1. Select the *Initialize Signature Pad* rule of the added view

When the form loads, the pad will be active and ready to capture signatures. Once you are ready to save the form, add the following actions in your Save rule:
1. Execute the *viewrule Get Data* of the Signature Pad view, this will get the data and put it in a hidden data label on the view.
1. Transfer it to the control where you want to handle it or the SmartObject property that will save it using a *Transfer Data* rule, using the *Data Label Signature Data* conrtol as the source.


### Redrawing the Signature
Use the view called *Signature Pad - View Signature*, located in the **Common/Signature Pad** category. As with the capture view, simply add the view to your form and initialize it by:
1. Adding (or editing) the *When the server loads the Form* rule
1. Add an action of *Execute a server rule*
1. Select the *Initialize Signature View Pad* rule of the added view

Prepare the form for rendering<br>
The script that redraws the signature expects the control to be labeled in a certain way, but that creates challenges when using the view multiple times on a single form. To allow the dynamic controls to be accessed by the scripts, some prep work needs to happen and that is done on using the *viewrule Prepare for Set Data* rule on the view. However, this needs to be done just once on the form, so this can't be bundled in the view's Initialze method. So after all the redraw views' Initialize rules have fired, call just the first view's *viewrule Prepare to Draw Signature* rule:<br>
![Mapping](https://github.com/K2-US-TS/Images/blob/master/Documentation/Signature%20Pad/Redraw%20Sig.png?raw=true)

When the saved data from the capture action becomes available, either when the form/view loads it or when the action completed that pulls this data:
1. Transfer it from the source control to the *Signature Pad - View Signature* view's control called *Data Label Saved Signature Data*
1. Execute the **viewrule Draw Signature** on the *Signature Pad - View Signature* view

There are also two forms called *Signature Pad Implementation* and *Signature Pad Implementation - Multiple Redraw* in the same category where the views are located, showing two working implementations that can be used for reference.

##### Scaling
The Redraw view has the option to scale the signature. This is useful when you have multiple signatures that you want to show on a read-only or additional approval forms. use the Scale parameter to adjust this, any decimal value will work, i.e. 0.5 or 0.25. You will need to set your own height and width if you want the pad's size to shrink as well.


### Styling and Customization
#### Height and Width
The Signature Pad's width and height is set using the following parameters:
1. **PadHeight**: Default of 200
1. **PadWidth**: Default of 550

Update the parameters before or as part of the Views' Initialize events and it will resize.

#### Pen Color
The color of the pen's "ink" can be set by setting the **PenColor** parameter. It uses RGB values, the default being 0,0,0 (Black).

#### Canvas Color
The color of the canvas can be changed from the default white (255,255,255,0.5) to the color of choice, using the RGB format, by setting the **CanvasColor** parameter. Note that in the case of the canvas, the opacity can also be set.

Additional styling can be done using CSS. The control is in essence just a canvas element with a class called *signature-pad* tied to it. You can create any custom styling for this class and include it in the form, the canvas will render accordingly. Here is an example:

[Signature Pad Styling](http://szimek.github.io/signature_pad/css/signature-pad.css)
