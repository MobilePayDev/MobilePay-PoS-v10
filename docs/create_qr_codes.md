# <a name="create_qr_codes"></a>Create QR codes

The correct format for creating a QR code for Point of Sale is: mobilepaypos://pos?id=<beaconId>&source=qr 
### Examples: 
- mobilepaypos://pos?id=123456789012345&source=qr 
- mobilepaypos://pos?id=cd851cd7-8398-4a03-83c1-391a2f5cb71c&source=qr

Please be aware that it is all lowercase. The POS beacon id is either a 15 digit number (no letters) or a GUID.
The format is both for sandbox and production environment.  

If yo want to print the QR code we have a template for MobilePay stickers on the [MobilePay website](https://www.mobilepay.dk/materialebank/marketingmateriale/klistermaerker/mobilepay-klistermaerker-m-plads-til-qr)
