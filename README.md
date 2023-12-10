#include <iostream>
#include <fstream>
#include <vector>
#include <algorithm>

using namespace std;

// Definición de la estructura de un libro
struct Libro {
    string titulo;
    string autor;
    int annoPublicacion;
    double precio;
    int cantidadStock;
};

// Funciones para realizar operaciones en el inventario
void agregarLibro(vector<Libro>& inventario);
void mostrarInventario(const vector<Libro>& inventario);
void buscarLibro(const vector<Libro>& inventario);
void venderLibro(vector<Libro>& inventario);
void ordenarInventario(vector<Libro>& inventario);
void guardarInventario(const vector<Libro>& inventario);
void cargarInventario(vector<Libro>& inventario);

int main() {
    vector<Libro> inventario; // Vector que almacena los libros

    // Menú de opciones
    int opcion;
    do {
        cout << "\n=== Menú de Opciones ===\n";
        cout << "1. Agregar Libro\n";
        cout << "2. Mostrar Inventario\n";
        cout << "3. Buscar Libro\n";
        cout << "4. Vender Libro\n";
        cout << "5. Ordenar Inventario\n";
        cout << "6. Guardar Datos\n";
        cout << "7. Cargar Datos\n";
        cout << "8. Salir\n";
        cout << "Ingrese la opción deseada: ";
        cin >> opcion;

        switch (opcion) {
            case 1:
                agregarLibro(inventario);
                break;
            case 2:
                mostrarInventario(inventario);
                break;
            case 3:
                buscarLibro(inventario);
                break;
            case 4:
                venderLibro(inventario);
                break;
            case 5:
                ordenarInventario(inventario);
                break;
            case 6:
                guardarInventario(inventario);
                break;
            case 7:
                cargarInventario(inventario);
                break;
            case 8:
                cout << "Saliendo del programa. ¡Hasta luego!\n";
                break;
            default:
                cout << "Opción no válida. Intente de nuevo.\n";
        }

    } while (opcion != 8);

    return 0;
}

// Función para agregar un libro al inventario
void agregarLibro(vector<Libro>& inventario) {
    Libro nuevoLibro;
    cout << "Ingrese el título del libro: ";
    cin.ignore();
    getline(cin, nuevoLibro.titulo);
    cout << "Ingrese el autor del libro: ";
    getline(cin, nuevoLibro.autor);
    cout << "Ingrese el año de publicación del libro: ";
    cin >> nuevoLibro.annoPublicacion;
    cout << "Ingrese el precio del libro: ";
    cin >> nuevoLibro.precio;
    cout << "Ingrese la cantidad en stock del libro: ";
    cin >> nuevoLibro.cantidadStock;

    inventario.push_back(nuevoLibro);

    cout << "Libro agregado al inventario.\n";
}

// Función para mostrar el inventario de libros
void mostrarInventario(const vector<Libro>& inventario) {
    if (inventario.empty()) {
        cout << "El inventario está vacío.\n";
        return;
    }

    cout << "\n=== Inventario de Libros ===\n";
    for (const auto& libro : inventario) {
        cout << "Título: " << libro.titulo << "\n";
        cout << "Autor: " << libro.autor << "\n";
        cout << "Año de Publicación: " << libro.annoPublicacion << "\n";
        cout << "Precio: $" << libro.precio << "\n";
        cout << "Cantidad en Stock: " << libro.cantidadStock << "\n\n";
    }
}

// Función para buscar un libro en el inventario
void buscarLibro(const vector<Libro>& inventario) {
    if (inventario.empty()) {
        cout << "El inventario está vacío. No hay libros para buscar.\n";
        return;
    }

    string criterio;
    cout << "Ingrese el título, autor u otra característica para buscar: ";
    cin.ignore();
    getline(cin, criterio);

    bool encontrado = false;
    for (const auto& libro : inventario) {
        // Se realiza una búsqueda parcial, es decir, se busca si el criterio está contenido en el título o autor
        if (libro.titulo.find(criterio) != string::npos || libro.autor.find(criterio) != string::npos) {
            cout << "Libro encontrado:\n";
            cout << "Título: " << libro.titulo << "\n";
            cout << "Autor: " << libro.autor << "\n";
            cout << "Año de Publicación: " << libro.annoPublicacion << "\n";
            cout << "Precio: $" << libro.precio << "\n";
            cout << "Cantidad en Stock: " << libro.cantidadStock << "\n";
            encontrado = true;
            break;
        }
    }

    if (!encontrado) {
        cout << "Libro no encontrado en el inventario.\n";
    }
}

// Función para vender un libro y actualizar el inventario
void venderLibro(vector<Libro>& inventario) {
    if (inventario.empty()) {
        cout << "El inventario está vacío. No hay libros para vender.\n";
        return;
    }

    mostrarInventario(inventario);

    int indice;
    cout << "Ingrese el número de libro que desea vender: ";
    cin >> indice;

    if (indice >= 1 && indice <= inventario.size()) {
        int cantidadVenta;
        cout << "Ingrese la cantidad que desea vender: ";
        cin >> cantidadVenta;

        if (cantidadVenta <= inventario[indice - 1].cantidadStock) {
            inventario[indice - 1].cantidadStock -= cantidadVenta;
            double totalVenta = cantidadVenta * inventario[indice - 1].precio;
            cout << "Venta realizada. Total: $" << totalVenta << "\n";
        } else {
            cout << "No hay suficiente stock para realizar la venta.\n";
        }
    } else {
        cout << "Número de libro no válido.\n";
    }
}

// Función para ordenar el inventario por título
bool compararPorTitulo(const Libro& libro1, const Libro& libro2) {
    return libro1.titulo < libro2.titulo;
}

// Función para ordenar el inventario por autor
bool compararPorAutor(const Libro& libro1, const Libro& libro2) {
    return libro1.autor < libro2.autor;
}

// Función para ordenar el inventario por año de publicación
bool compararPorAnnoPublicacion(const Libro& libro1, const Libro& libro2) {
    return libro1.annoPublicacion < libro2.annoPublicacion;
}

// Función para ordenar el inventario
void ordenarInventario(vector<Libro>& inventario) {
    if (inventario.empty()) {
        cout << "El inventario está vacío. No hay libros para ordenar.\n";
        return;
    }

    int opcionOrdenamiento;
    cout << "Seleccione el criterio de ordenamiento:\n";
    cout << "1. Por Título\n";
    cout << "2. Por Autor\n";
    cout << "3. Por Año de Publicación\n";
    cout << "Ingrese la opción deseada: ";
    cin >> opcionOrdenamiento;

    switch (opcionOrdenamiento) {
        case 1:
            sort(inventario.begin(), inventario.end(), compararPorTitulo);
            cout << "Inventario ordenado por título.\n";
            break;
        case 2:
            sort(inventario.begin(), inventario.end(), compararPorAutor);
            cout << "Inventario ordenado por autor.\n";
            break;
        case 3:
            sort(inventario.begin(), inventario.end(), compararPorAnnoPublicacion);
            cout << "Inventario ordenado por año de publicación.\n";
            break;
        default:
            cout << "Opción de ordenamiento no válida.\n";
    }
}

// Función para guardar el inventario en un archivo
void guardarInventario(const vector<Libro>& inventario) {
    if (inventario.empty()) {
        cout << "El inventario está vacío. No hay datos para guardar.\n";
        return;
    }

    ofstream archivo("inventario.txt");
    if (archivo.is_open()) {
        for (const auto& libro : inventario) {
            archivo << libro.titulo << "|" << libro.autor << "|" << libro.annoPublicacion
                    << "|" << libro.precio << "|" << libro.cantidadStock << "\n";
        }
        archivo.close();
        cout << "Datos guardados correctamente en el archivo 'inventario.txt'.\n";
    } else {
        cout << "No se pudo abrir el archivo para guardar datos.\n";
    }
}

// Función para cargar el inventario desde un archivo
void cargarInventario(vector<Libro>& inventario) {
    ifstream archivo("inventario.txt");
    if (archivo.is_open()) {
        inventario.clear(); // Limpiar el inventario actual antes de cargar nuevos datos

        string linea;
        while (getline(archivo, linea)) {
            Libro libro;
            size_t pos = 0;
            pos = linea.find("|");
            libro.titulo = linea.substr(0, pos);
            linea.erase(0, pos + 1);
            pos = linea.find("|");
            libro.autor = linea.substr(0, pos);
            linea.erase(0, pos + 1);
            pos = linea.find("|");
            libro.annoPublicacion = stoi(linea.substr(0, pos));
            linea.erase(0, pos + 1);
            pos = linea.find("|");
            libro.precio = stod(linea.substr(0, pos));
            linea.erase(0, pos + 1);
            libro.cantidadStock = stoi(linea);

            inventario.push_back(libro);
        }

        archivo.close();
        cout << "Datos cargados correctamente desde el archivo 'inventario.txt'.\n";
    } else {
        cout << "No se pudo abrir el archivo para cargar datos.\n";
    }
}
