# Zatca Phase 1 QrCode Data Generation
ZATCA E-invoicing Phase 1 QrCode data generation Samples with various programming languages 

# C#.Net

first we need to create an extension class for memory streem (optional step for simplify code)


    public static class MemoryStreamExtensions
    {
        public static void Append(this MemoryStream stream, byte value)
        {
            stream.Append(new[] { value });
        }
        public static void Append(this MemoryStream stream, byte[] values)
        {
            stream.Write(values, 0, values.Length);
        }
    }
    

then we can do the following step 


    MemoryStream stream = new MemoryStream();
	
    // Tag Length value Tuples
    List<Tuple<int,string,string>> listTuple = new List<Tuple<int, string, string>>();
    listTuple.Add(new Tuple<int, string, string>(1, "Seller", "Salah Hospital"));
    listTuple.Add(new Tuple<int, string, string>(2, "VatNumber", "31012239350000311123"));
    listTuple.Add(new Tuple<int, string, string>(3, "TimeStamp", "2023-01-01"));
    listTuple.Add(new Tuple<int, string, string>(4, "InvoiceTotal", "1150.00"));
    listTuple.Add(new Tuple<int, string, string>(5, "VatTotal", "150"));
    foreach ( Tuple<int, string, string> msg in listTuple)
        {
	// Tag into byte
        stream.Append((byte)msg.Item1);
	
	// convert value into utf8 byte array
        byte[] utf16Bytes = utf8.GetBytes(msg.Item3);
	
	// get length of the value byte array and convert it into byte
        stream.Append((byte)Convert.ToInt32(utf16Bytes.Length));
	
	// append value utf8 byte array into stream
        stream.Append(utf16Bytes);
	
        }
    byte[] data = stream.ToArray();  
    // convert into base 64 string
    var x = Convert.ToBase64String(data);
    // we can use x as qr data
    // x = AQ5TYWxhaCBIb3NwaXRhbAIUMzEwMTIyMzkzNTAwMDAzMTExMjMDCjIwMjMtMDEtMDEEBzExNTAuMDAFAzE1MA==
    

# Dart 


    import 'dart:typed_data';
    import 'dart:convert';
    
    void main() {
      BytesBuilder bytesBuilder = BytesBuilder();
    
      List<Tlv> lst = [];
      lst.add(new Tlv(tag: 1, key: "Seller", value: "Salah Hospital"));
      lst.add(new Tlv(tag: 2, key: "VatNumber", value: "31012239350000311123"));
      lst.add(new Tlv(tag: 3, key: "Timestamp", value: "2023-01-01"));
      lst.add(new Tlv(tag: 4, key: "InvoiceTotal", value: "1150.00"));
      lst.add(new Tlv(tag: 5, key: "VatTotal", value: "150"));
    
      lst.forEach((element) {
        bytesBuilder.addByte(element.tag);
        List<int> sellerNameBytes = utf8.encode(element.value);
        bytesBuilder.addByte(sellerNameBytes.length);
        bytesBuilder.add(sellerNameBytes);
      });
    
      Uint8List qrCodeAsBytes = bytesBuilder.toBytes();
      final Base64Encoder b64Encoder = Base64Encoder();
      print(b64Encoder.convert(qrCodeAsBytes));
    }
    
    class Tlv {
      int tag;
      String key;
      String value;
    
      Tlv({
        required this.tag,
        required this.key,
        required this.value,
      });
    }


	

	

# You can Test it with zatca sdk by the following command.

fatoorah.bat validateqr -qr AQ5TYWxhaCBIb3NwaXRhbAIUMzEwMTIyMzkzNTAwMDAzMTExMjMDCjIwMjMtMDEtMDEEBzExNTAuMDAFAzE1MA==


