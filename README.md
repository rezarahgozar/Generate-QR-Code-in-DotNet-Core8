# QR Code Generator with .NET Core Minimal API

This project demonstrates how to generate custom QR codes with the **QRCoder** library using a .NET Core Minimal API. It includes functionality to add logos, change QR code colors, and return the generated QR code as a Base64 string for easy embedding in web applications.

## Features

- Generate QR codes with custom text.
- Add logos to the center of the QR code.
- Customize QR code colors (foreground and background).
- Return the QR code as a Base64 string ready for embedding in HTML.
- Supports high-quality output using `System.Drawing`.

## Getting Started

### Prerequisites

- [.NET Core SDK](https://dotnet.microsoft.com/download) (version 6.0 or higher)
- [Visual Studio](https://visualstudio.microsoft.com/) or any code editor
- Install **QRCoder** and **System.Drawing.Common** NuGet packages

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/qrcode-generator-dotnet-core.git
   

2. Navigate to the project directory:

   ```bash
   cd qrcode-generator-dotnet-core
   ```

3. Install the required NuGet packages:

   ```bash
   dotnet add package QRCoder
   dotnet add package System.Drawing.Common
   ```

### Setup

1. **Create a new ASP.NET Core Web API project** by following these steps:

   - Open Visual Studio and click on **Create New Project**.
   - Select **ASP.NET Core Web API** and click **Next**.
   - Configure the project settings (name, location, framework version) and click **Create**.

2. **Add the QRCoder and System.Drawing.Common packages** using the NuGet Package Manager:

   ```bash
   Install-Package QRCoder
   Install-Package System.Drawing.Common
   ```

3. **Add the `QRCode.cs` class**:
   
   Copy the `QRCode` class from the code snippet above into your project under the `QRCodePOC` namespace. This class handles QR code generation with custom logos and colors.

4. **Create the wwwroot folder**:

   Right-click on your project name in Visual Studio and add a new folder called `wwwroot/img`. Add a logo image (e.g., `aircodlogo.jpg`) to this folder for use in QR codes.

5. **Update the `Program.cs`**:

   Add the following code to the `Program.cs` file to create an API endpoint that generates a QR code:

   ```csharp
   using QRCoder;
   using System.Drawing;
   using System.Drawing.Imaging;
   using QRCodePOC;

   var builder = WebApplication.CreateBuilder(args);
   var app = builder.Build();

   app.MapGet("/GenerateQRCode", (string text) =>
   {
       var guid = Guid.NewGuid().ToString();
       QRCodeGenerator qrGenerator = new QRCodeGenerator();
       QRCodeData qrCodeData = qrGenerator.CreateQrCode(text + "\r\n" + guid, QRCodeGenerator.ECCLevel.Q);
       QRCode qrCode = new QRCode(qrCodeData);
       Bitmap qrCodeImage = qrCode.GetGraphic(20);

       Bitmap logoImage = new Bitmap(@"wwwroot/img/yourlogo.jpg");

       using (Bitmap qrCodeAsBitmap = qrCode.GetGraphic(20, Color.Black, Color.WhiteSmoke, logoImage))
       {
           using (MemoryStream ms = new MemoryStream())
           {
               qrCodeAsBitmap.Save(ms, ImageFormat.Png);
               string base64String = Convert.ToBase64String(ms.ToArray());
               return "data:image/png;base64," + base64String;
           }
       }
   });

   app.Run();
   ```

### Usage

1. **Run the API**:

   ```bash
   dotnet run
   ```

2. **Access the endpoint**:

   Open a browser and navigate to:

   ```
   http://localhost:5000/GenerateQRCode?text=YourCustomText
   ```

   The API will return a Base64 string representing the generated QR code, which you can embed in HTML using:

   ```html
   <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABVYAAAMACAIAAAB3G5..." />
   ```

### Customization

You can customize the QR code with the following options in the `GetGraphic` method:

- **Foreground and background colors**: Change the colors by passing different `Color` parameters.
- **Logo image**: Add a logo to the center of the QR code by providing the path to the logo image.
- **Pixel size**: Adjust the size of the QR code by modifying the pixel size argument in `GetGraphic()`.

### Example

Here’s an example request:

```
http://localhost:5000/GenerateQRCode?text=HelloWorld
```

The response is a Base64-encoded string for the generated QR code with "HelloWorld" and a GUID.

### License

This project is licensed under the MIT License.
```

Feel free to replace the GitHub link in the clone command with your repository's URL!
