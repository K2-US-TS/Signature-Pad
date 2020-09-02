# Signature Pad for SmartForms
## Introduction
With the Signature Pad, you can capture signatures as part of the approval steps of the request. One can scribble using your mouse or finger on touch-screen devices. You can also redraw the signature for readonly views of the request.


## Included Components
The Signature Pad component has two views, one allowing you to capture signatures and one to redraw these in a readonly manner. Both can also reside on a form at the same time. These make use of a cool third-party script to handle the signature action and data interpretation. There are some additional properties that can be set but I didn't see the need for that right now, details here:

[GitHub - Signature Pad](https://github.com/szimek/signature_pad)


## Using the Signature Pad Views in your Forms
### Capturing a Signature
Use the view called *Signature Pad - Sign*, located in the **Common/Signature Pad** directory. Simply add the view to your form and initialize it by:
1. Adding (or editing) the *When the server loads the Form* rule
1. Add an action of *Execute a server rule*
1. Select the *Initialize Signature Pad* rule of the added view

When the form loads, the pad will be active and ready to capture signatures. Once you are ready to save the form, add the following actions in your Save rule:
1. Execute the *viewrule Get Data* of the Signature Pad view, this will get the data and put it in a hidden data label on the view.
1. Transfer it to the control where you want to handle it or the SmartObject property that will save it using a *Transfer Data* rule, using the *Data Label Signature Data* conrtol as the source.


### Redrawing the Signature
Use the view called *Signature Pad - View Signature*, located in the **Common/Signature Pad** directory. As with the capture view, simply add the view to your form and initialize it by:
1. Adding (or editing) the *When the server loads the Form* rule
1. Add an action of *Execute a server rule*
1. Select the *Initialize Signature View Pad* rule of the added view

When the saved data from the capture action becomes available, either when the form/view loads it or when the action completed that pulls this data:
1. Transfer it from the source control to the *Signature Pad - View Signature* view's control called *Data Label Saved Signature Data*
1. Execute the viewrule Set Data on the *Signature Pad - View Signature* view

There is also a form called *Signature Pad Implementation* in the same category where the views are location that shows a working implementation.


### Styling and Customization
Currently, the Signature Pad has very limited theming applied to it. It is in essence just a canvas element but has a class defined for it called *signature-pad*. You can create any custom styling for this class and include it in the form, the canvas will render accordingly. Here is an example:

[Signature Pad Styling](http://szimek.github.io/signature_pad/css/signature-pad.css)

The only easy customization included right now is to set the pad's height and width. Pass your custom values into parameters when the views are initialized and it will scale accordingly. Just ensure that you use the same settings for both the capture and redraw views, else the signature could be truncated. 
