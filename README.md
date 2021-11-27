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
    
# You can Test it with zatca sdk by the following command.

fatoorah.bat validateqr -qr AQ5TYWxhaCBIb3NwaXRhbAIUMzEwMTIyMzkzNTAwMDAzMTExMjMDCjIwMjMtMDEtMDEEBzExNTAuMDAFAzE1MA==


