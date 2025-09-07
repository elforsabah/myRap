import base64

# Specify the path to your PDF file
pdf_path = r"C:\Users\elvis.forsab\Desktop\MyWork\MP\TEST_Terminal_App\PDF_BASE64\TestPDF.pdf"

# Read the PDF file in binary mode
with open(pdf_path, "rb") as pdf_file:
    # Encode the PDF file to Base64 (keep as bytes, like ABAP xstring)
    base64_xstring = base64.b64encode(pdf_file.read())  # <-- returns bytes

# Save the Base64 bytes into a file (binary mode)
with open("output_base64.bin", "wb") as bin_file:
    bin_file.write(base64_xstring)

print("Base64 XSTRING (bytes) saved to output_base64.bin")
