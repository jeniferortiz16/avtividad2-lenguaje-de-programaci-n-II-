# avtividad2-lenguaje-de-programaci-n-II-
#include <windows.h>
#include <sql.h>
#include <sqlext.h>
#include <iostream>
#include <iomanip>
using namespace std;
int main() {
    SQLHENV hEnv;
    SQLHDBC hDbc;
    SQLRETURN ret;
    ret = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &hEnv);
    ret = SQLSetEnvAttr(hEnv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0);
    ret = SQLAllocHandle(SQL_HANDLE_DBC, hEnv, &hDbc);
    ret = SQLConnect(hDbc, (SQLWCHAR*)L"sql", SQL_NTS, (SQLWCHAR*)L"Username", SQL_NTS, (SQLWCHAR*)L"Password", SQL_NTS);
    if (ret == SQL_SUCCESS || ret == SQL_SUCCESS_WITH_INFO) {
        cout << "Conectado a la base de datos exitosamente." << endl;
        SQLHSTMT hStmt;
        ret = SQLAllocHandle(SQL_HANDLE_STMT, hDbc, &hStmt);
       wcout << L"+-------+----------+-----------------+---------------+-----------+-----+------------------+------------------------+ " << endl;
       wcout << L"|  No.  |  Nombre  |  Apellido pat.  | Apellido mat. | f. de nac | RFC | Centro supervisa | Prestacion combustible | " << endl;
       wcout << L"+-------+----------+-----------------+---------------+-----------+-----+------------------+------------------------+ " << endl;

        ret = SQLExecDirect(hStmt, (SQLWCHAR*)L"SELECT * FROM directivo", SQL_NTS);
        if (ret == SQL_SUCCESS || ret == SQL_SUCCESS_WITH_INFO)
        SQLFreeHandle(SQL_HANDLE_STMT, hStmt);
    }
    else {
        cout << "Fallo la conexion a la base de datos" << endl;
    }
    SQLDisconnect(hDbc);
    SQLFreeHandle(SQL_HANDLE_DBC, hDbc);
    SQLFreeHandle(SQL_HANDLE_ENV, hEnv);
    return 0;
}
