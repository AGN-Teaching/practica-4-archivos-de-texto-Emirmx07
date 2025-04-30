def cargar_diccionario(nombre_archivo):
    """Carga palabras correctas desde un archivo a un diccionario."""
    try:
        with open(nombre_archivo, 'r', encoding='utf-8') as archivo:
            palabras = {}
            for linea in archivo:
                palabra = linea.strip().lower()
                palabras[palabra] = 0  # Valor irrelevante
            return palabras
    except FileNotFoundError:
        print(f"Error: Archivo {nombre_archivo} no encontrado.")
        return None

def limpiar_palabra(palabra):
    """Elimina signos de puntuación alrededor de una palabra."""
    signos = '.,;:¿?¡!"\'()[]{}'
    while len(palabra) > 0 and palabra[0] in signos:
        palabra = palabra[1:]
    while len(palabra) > 0 and palabra[-1] in signos:
        palabra = palabra[:-1]
    return palabra.lower() 

def verificar_ortografia(archivo_usuario, diccionario):
    """Busca palabras incorrectas en un archivo."""
    palabras_incorrectas = []
    try: 
        with open(archivo_usuario, 'r', encoding='utf-8') as archivo:
            for linea in archivo:
                for palabra in linea.split():
                    palabra_limpia = limpiar_palabra(palabra)
                    if palabra_limpia and palabra_limpia not in diccionario:
                        palabras_incorrectas.append(palabra_limpia)
    except FileNotFoundError:
        print(f"Error: No se pudo abrir {archivo_usuario}")
        return None
    return palabras_incorrectas

def main():
    # Cargar palabras correctas
    diccionario = cargar_diccionario("palabras.txt")
    if diccionario is None:
        return

    # Pedir archivo al usuario
    nombre_archivo = input("Ingrese el nombre del archivo a verificar: ")

    # Verificar ortografía
    errores = verificar_ortografia(nombre_archivo, diccionario)

    # Mostrar resultados
    if errores is not None:
        if not errores:
            print("No se encontraron errores de ortografía.")
        else:
            print("\nPalabras con posible error:")
            for palabra in errores:
                print(f"- {palabra}")
            print(f"\nTotal de errores: {len(errores)}")

if _name_ == "_main_":
    main()
