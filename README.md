# duino bypass blocked pool

step 1. <br>
cari dan install VPN gatisan di laptop, PC atau HP (kalau mau).

step 2. <br>
nyalakan VPN dan kemudian akses duino pool : https://server.duinocoin.com/getPool

step 3. <br>
copy text yang muncul baru di browser, format JSON. (jangan pakai JSON dalam contoh ini)<br>
contoh : {"ip":"51.159.175.20","name":"42-pool-2","port":6043,"server":"duino-master-1","success":true}

step4. <br>
akses https://www.freeformatter.com/json-escape.html <br> kemudian paste-kan JSON data yang abang punya di browser kemudian pencet button 'escape'.

step 5. <br>
tambah kan code dan JSON data yang sudah di escape, <br>

    //begin - tambahan code
    if (input=="" || input==NULL){
      input ="JSON_DATA_YANG_SUDAH_DI_ESCAPE_MASUKIN_SINI";
    }
    //end - tambahan code

step 6. <br>
gabung code diatas kedalam code utama ESP cari method "Void UpdatePool()". hasil akhir akan seperti ini. <br> JANGAN PAKAI POOL INI, PAKAI POOL YANG BARU KALIAN BUAT.


    // Communication Functions
    void UpdatePool() {
      String input = "";
      int waitTime = 1;
      int poolIndex = 0;
      int poolSize = sizeof(POOLPICKER_URL) / sizeof(char*);

      while (input == "") {
        Serial.println("Fetching mining node from the poolpicker in " + String(waitTime) + "s");
        input = httpGetString(POOLPICKER_URL[poolIndex]);
        poolIndex += 1;

        // Check if pool index needs to roll over
        if (poolIndex >= poolSize) {
          poolIndex %= poolSize;
          delay(waitTime * 1000);
        
        //begin - tambahan code
        if (input=="" || input==NULL){
          input ="{\"ip\":\"51.159.175.20\",\"name\":\"42-pool-2\",\"port\":6043,\"server\":\"duino-master-1\",\"success\":true}";
        }
        //end - tambahan code
        
          // Increase wait time till a maximum of 32 seconds (addresses: Limit connection requests on failure in ESP boards #1041)
          waitTime *= 2;
          if ( waitTime > 32 )
            waitTime = 32;
        }
      }



      // Setup pool with new input
      UpdateHostPort(input);
    }
    
    
   NOTE: <br>
   ini akan switching otomatis, pilihan pertama akan pakai pool resmi, kalau ndak nemu baru pakai pool karbitan.
