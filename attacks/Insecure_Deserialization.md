# Insecure derialization

- Pri logine sa uklada do cookie suboru email pouzivatela a zaroven aj informacia, ci je pouzivatel adminom.
- Tato informacia je zakodovana v base64, takze nie je priamo viditelna.
- Zoberieme z cookies suborov tento string a dekodujeme ho.
- V zdekodovanom stringu zmenime hodnotu isAdmin z false na true.
- Vlozime string naspat do cookie suborov.
- Pri zhliadnuti profilu mozeme vidiet, ze sa nam zmenila rola z usera na admina.