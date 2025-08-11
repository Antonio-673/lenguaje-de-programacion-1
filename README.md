/****************************************************************************

codigo que genera rfc de una persona
autor:Juan Antonio Valenzuela Castillo
fecha:10-08-2025

*****************************************************************************/
#include <iostream>
#include <vector>
#include <string>

//diccionario de palabras no permitidas
const std:: vector<std::string> palabrasProhibidas = {
    "pene","popo","puto"
};

//verificar y modificar palabras del diccionario
std::string corregirRFC(const std::string& rfc) {
    for (const std::string& palabra : palabrasProhibidas) {
        if (rfc == palabra) {
            return rfc.substr(0, 3) + "X"; //reemplazar la ultima letra por ´X´
        }
    }
    return rfc;
}

//funcion para obtener la primera vocal interna de una cadena
char obtenerPrimeraVocalInterna(const std::string& str) {
    for (size_t i = 1; i < str.length(); ++i) {
        char c = str[i];
        if (c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U' )
        return c;
    }
    return 'x'; //si no encuentra ninguna vocal interna, se usa una x
}

//funcion principal para calcular rfc
std::string calcularRFC(const std::string& nombre, const std::string& apellidoPaterno, const std::string& apellidoMaterno, const std::string& fechaNacimiento) {
    
    //se declara la variable donde guardaremos el rfcstd
    std::string rfc;
    
    //se obtine la letra inicial y la primera vocal interna del apellido apellido Paterno
    char letraInicial = apellidoPaterno[0];
    char PrimeraVocalInterna = obtenerPrimeraVocalInterna(apellidoPaterno);
    
    //se obtine la inicial del apellido materno o se usa 'X' si no existe
    char inicialApellidoMaterno = (!apellidoMaterno.empty()) ? apellidoMaterno[0] : 'x';
    
    //se obtiene la inicial del primer nombre o se usa 'X' si no existe
    char inicialNombre = nombre[0];
    
    //se obtine los dos ultimos dijitos del año de fechaNacimiento
    std::string anio = fechaNacimiento.substr(2, 2);
    
    //se obtiene los dos dijitos del año de fechaNacimiento
    std::string mes = fechaNacimiento.substr(5, 2);
    std::string dia = fechaNacimiento.substr(8, 2);
    
    //se construye el RFC sumando el texto de la informacion que se proporciona
    rfc = letraInicial;
    rfc += PrimeraVocalInterna;
    rfc += inicialApellidoMaterno;
    rfc += inicialNombre;
    
    //aqui se compara con la BD de palabras mal formadas y se sustituye con la letra 'X'
    rfc = corregirRFC(rfc);
    
    //si hay correccion se continua generando rfc
    rfc += anio;
    rfc += mes;
    rfc += dia;
    return rfc;
}

int main() {
    //envia a consola los datos que requiere para solicitar nombre, apellido y fecha de fechaNacimiento
    std::string nombre, apellidoPaterno, apellidoMaterno, fechaNacimiento;
    std::cout <<"Introduce tu nombre: ";
    std::getline(std::cin, nombre);
    std::cout <<"Introduce tu apellido paterno: ";
    std::getline(std::cin, apellidoPaterno);
    std::cout <<"Introduce tu apellido materno (si no cuenta con apellido materno, precione Enter: ";
    std::getline(std::cin, apellidoMaterno);
    std::cout <<"Introduce la fecha de nacimiento en el siguiente formato (YYYY-MM-DD): ";
    std::getline (std::cin, fechaNacimiento);
    std::string rfc = calcularRFC(nombre, apellidoPaterno, apellidoMaterno, fechaNacimiento);
    std::cout << "RFC: " << rfc <<std::endl;
    
    return 0;
}
