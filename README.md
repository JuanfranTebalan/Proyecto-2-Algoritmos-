# Proyecto-2-Algoritmos-
Se presentan 2 Ejercicios hechos en C++ y Python 

cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>

using namespace std;

struct Producto {
    string nombre;
    string codigo;
    float precio;
    string proveedor;
    int existencia;
    char estado;
    float descuento;
};

// Función para agregar un producto al archivo
void agregarProducto(vector<Producto>& productos, ofstream& archivo) {
    Producto producto;
    cout << "Ingrese el nombre del producto: ";
    cin >> producto.nombre;
    cout << "Ingrese el código del producto: ";
    cin >> producto.codigo;
    // Verificar si el código ya existe
    for (const Producto& p : productos) {
        if (p.codigo == producto.codigo) {
            cout << "El código de producto ya existe." << endl;
            return;
        }
    }
    cout << "Ingrese el precio del producto: ";
    cin >> producto.precio;
    cout << "Ingrese el proveedor: ";
    cin >> producto.proveedor;
    cout << "Ingrese la existencia del producto: ";
    cin >> producto.existencia;
    cout << "Ingrese el estado (A = Aprobado, R = Reprobado): ";
    cin >> producto.estado;
    cout << "Ingrese el descuento del producto: ";
    cin >> producto.descuento;
    
    productos.push_back(producto);
    archivo << producto.nombre << " " << producto.codigo << " " << producto.precio << " " << producto.proveedor << " " << producto.existencia << " " << producto.estado << " " << producto.descuento << endl;
    cout << "Producto agregado con éxito." << endl;
}

// Función para buscar un producto por código o nombre
void buscarProducto(const vector<Producto>& productos, const string& clave) {
    for (const Producto& producto : productos) {
        if (producto.codigo == clave || producto.nombre.find(clave) != string::npos) {
            cout << "Nombre: " << producto.nombre << endl;
            cout << "Código: " << producto.codigo << endl;
            cout << "Precio: " << producto.precio << endl;
            cout << "Proveedor: " << producto.proveedor << endl;
            cout << "Existencia: " << producto.existencia << endl;
            cout << "Estado: " << producto.estado << endl;
            cout << "Descuento: " << producto.descuento << endl;
            return;
        }
    }
    cout << "Producto no encontrado." << endl;
}

int main() {
    vector<Producto> productos;
    ifstream archivoEntrada("productos.txt");
    string nombreArchivo = "productos.txt";
    if (!archivoEntrada) {
        ofstream archivoSalida(nombreArchivo);
        archivoSalida.close();
    } else {
        string nombre, codigo, proveedor, estado;
        float precio, descuento;
        int existencia;
        while (archivoEntrada >> nombre >> codigo >> precio >> proveedor >> existencia >> estado >> descuento) {
            Producto producto = {nombre, codigo, precio, proveedor, existencia, estado[0], descuento};
            productos.push_back(producto);
        }
        archivoEntrada.close();
    }
    
    int opcion;
    while (true) {
        cout << "Menú:" << endl;
        cout << "1. Agregar producto" << endl;
        cout << "2. Buscar producto" << endl;
        cout << "3. Salir" << endl;
        cout << "Seleccione una opción: ";
        cin >> opcion;
        
        if (opcion == 1) {
            ofstream archivoSalida(nombreArchivo, ios::app);
            agregarProducto(productos, archivoSalida);
            archivoSalida.close();
        } else if (opcion == 2) {
            string clave;
            cout << "Ingrese el código o nombre del producto: ";
            cin >> clave;
            buscarProducto(productos, clave);
        } else if (opcion == 3) {
            break;
        } else {
            cout << "Opción no válida." << endl;
        }
    }
    
    return 0;
}
