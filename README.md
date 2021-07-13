# Student-MIS-Sqlite3
#include <stdio.h>
#include <stdlib.h>
#include <sqlite3.h>
#include <iostream>
#include <string>
using namespace std;
static int callback(void* NotUsed, int argc, char** argv, char** azColName) {
    int i;
    for (i = 0; i < argc; i++) {
        printf("%s = %s\n", azColName[i], argv[i] ? argv[i] : "NULL");
    }
    printf("\n");
    return 0;
}

void insertindb(string cnic, string name, string nationality, string religion, string address, string ph) {
    sqlite3* db;
    char* zErrMsg = 0;
    int rc;
    string sql;

 

    rc = sqlite3_open("project.db", &db);

    sql = "INSERT INTO STDDMS (CNIC,NAME,NATIONALITY,RELIGION,ADDRESS,PHONE) "  \
        "VALUES(" + cnic + ", '" + name + "','" + nationality + "','" + religion + "', '" + address + "', '" + ph + "');";

    rc = sqlite3_exec(db, sql.c_str(), callback, 0, &zErrMsg);


    if (rc) {
        cout << "" << sqlite3_errmsg(db);
        cout << (0);
    }
    else {
        cout << "Opened database successfully\n";
    }
    sqlite3_close(db);



}

int main(int argc, char* argv[] ) {
    sqlite3* db;
    char* zErrMsg = 0;
    int rc;
    const char* sql;
    string cnic;
    string name;
    string nationality;
    string religion;
    string address;
    string ph;
    int x=1;
    /* Open database */
    rc = sqlite3_open("project.db", &db);

    cout << "Enter how many times you run the loop:";
    //cin >> x;
    while (x<3) {
        cout << "\n<----WELL COME STDDMS----> " << endl;
        cout << "enter your cnic =   ";
        cin >> cnic;
        cout << "enter your full name =   ";
        cin >> name;
        cout << "Nationality = ";
        cin >> nationality;
        cout << "Religion = ";
        cin >> religion;
        cout << "Enter your address = ";
        cin >> address;
        cout << "Enter your phone number = ";
        cin >> ph;
        insertindb(cnic, name, nationality, religion, address, ph);
        x++;
        //rc = sqlite3_exec(db, sql, callback, 0, &zErrMsg);
        sql = "SELECT* from STDDMS";
    }
    /* Execute SQL statement */
    rc = sqlite3_exec(db, sql, callback, 0, &zErrMsg);

    
   

    if (rc) {
        fprintf(stderr, "Can't open database: %s\n", sqlite3_errmsg(db));
        return(0);
    }
    else {
        fprintf(stdout, "Opened database successfully\n");
    }

    /* Create SQL statement */
    sql = "CREATE TABLE STDDMS("  \
        "CNIC CHAR(20) NOT NULL," \
        "NAME           CHAR(20)   NOT NULL," \
        "NATIONALITY         CHAR(20)," \
        "RELIGION            CHAR(20)," \
        "ADDRESS        CHAR(50)," \
        "PHONE         CHAR(20));";

    
    
    if (rc != SQLITE_OK) {
        fprintf(stderr, "SQL error: %s\n", zErrMsg);
        sqlite3_free(zErrMsg);
    }
    else {
        fprintf(stdout, "Table created successfully\n");
    }

    sqlite3_close(db);
}
