#include <ESP8266WiFi.h>
#include <MySQL_Connection.h>
#include <MySQL_Cursor.h>

const char* ssid     = "WIFI-SSID";
const char* password = "WIFI-PASSWORD";

byte mac_addr[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };

IPAddress server_addr(192,168,1,100);  // IP ของ MySQL server
char dbuser[] = "DB-USERNAME";         // MySQL username
char dbpassword[] = "DB-PASSWORD";       // MySQL password

// SELECT query (เงื่อนไขในการ Query ข้อมูล)
char query[] = "SELECT * FROM DBName.TableName WHERE id = '5' ";  //กำหนดชื่อฐานข้อมูลและชื่อตาราง  DBName.TableName

WiFiClient client;    
MySQL_Connection conn((Client *)&client);

void setup() {
  Serial.begin(115200);
  delay(10);

  // Connect to a WiFi network
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
      delay(500);
      Serial.print(".");
    }

  Serial.println("");
  Serial.print("WiFi connected to ");
  Serial.print("IP : ");
  Serial.println(WiFi.localIP());
  Serial.println("Your device is now online to internet.");
  Serial.println("");

 //MySQL Connection
  Serial.println("Connecting...");
  if (conn.connect(server_addr, 3306, dbuser, dbpassword)) {
    delay(1000);
    Serial.println("MySQL Connected.");
  }
  else
    Serial.println("Connection failed.");
  //conn.close();
  
}

void loop() {
  row_values *row = NULL;
  //String head_count ; 
  delay(10000);
  
  Serial.println("Selecting data.");

  MySQL_Cursor *cur_mem = new MySQL_Cursor(&conn);
  
  cur_mem->execute(query); // Execute the query
  column_names *columns = cur_mem->get_columns(); // Fetch the columns

  // Read the row (we are only expecting the one)
  do {
    row = cur_mem->get_next_row();
    if (row != NULL) {
      Serial.println(row->values[1]); //ค่าที่ SELECT ได้จากฐานข้อมูล
    }
  } while (row != NULL);
  delete cur_mem;  // Deleting the cursor also frees up memory used 
}
