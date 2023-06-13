# Esp32 Sqlite3 Con SD Card

### Se debe crear una base de datos
<img src="https://github.com/IDiegoUlises/Esp32-Sqlite3-Con-SD-Card/blob/main/images/Base-de-datos-size.jpg" width="1000" height="500" />

* Se debe crear una tabla usuarios
* Se debe crear una columna nombre
* Se debe crear una columna edad

### Conexion
<img src="https://github.com/IDiegoUlises/Esp32-Sqlite3-Con-SD-Card/blob/main/images/MicroSD-Con-SDCARD.jpg"  />

* El modulo incorpora un regulador de voltaje para usar 5v
* Se puede alimentar con 3.3V

### Codigo Simplificado Que funciona
```c++
#include <stdio.h>
#include <stdlib.h>
#include <sqlite3.h>
#include <SPI.h>
#include <FS.h>
#include "SD.h"

const char* data = "Callback function called";
static int callback(void *data, int argc, char **argv, char **azColName) {
  int i;
  Serial.printf("%s: ", (const char*)data);
  for (i = 0; i < argc; i++) {
    Serial.printf("%s = %s\n", azColName[i], argv[i] ? argv[i] : "NULL");
  }
  Serial.printf("\n");
  return 0;
}

int openDb(const char *filename, sqlite3 **db) {
  int rc = sqlite3_open(filename, db);
  if (rc) {
    Serial.printf("Can't open database: %s\n", sqlite3_errmsg(*db));
    return rc;
  } else {
    Serial.printf("Opened database successfully\n");
  }
  return rc;
}

char *zErrMsg = 0;
int db_exec(sqlite3 *db, const char *sql) {
  Serial.println(sql);
  long start = micros();
  int rc = sqlite3_exec(db, sql, callback, (void*)data, &zErrMsg);
  if (rc != SQLITE_OK) {
    Serial.printf("SQL error: %s\n", zErrMsg);
    sqlite3_free(zErrMsg);
  } else {
    Serial.printf("Operation done successfully\n");
  }
  Serial.print(F("Time taken:"));
  Serial.println(micros() - start);
  return rc;
}

void setup() {
  Serial.begin(115200);
  sqlite3 *db1;
  char *zErrMsg = 0;
  int rc;

  SPI.begin();
  SD.begin();

  sqlite3_initialize();

  // Open database 1
  if (openDb("/sd/user.db", &db1))
    return;

  rc = db_exec(db1, "SELECT nombre FROM usuarios;");
  if (rc != SQLITE_OK) {
    sqlite3_close(db1);
    return;
  }

  sqlite3_close(db1);

}

void loop() {
}
```
