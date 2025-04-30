def cargar_diccionario(nombre_archivo):
    """
    Carga las palabras correctas desde un archivo a un diccionario.
    Devuelve el diccionario y None si hay error.
    """
    try:
        with open(nombre_archivo, 'r', encoding='utf-8') as archivo:
            palabras = {}
            for linea in archivo:
                palabra = linea.strip().lower()
                palabras[palabra] = 0  # El valor no importa, solo las claves
            return palabras
    except FileNotFoundError:
        print(f"Error: No se pudo encontrar el archivo {nombre_archivo}")
        return None

def limpiar_palabra(palabra):
    """
    Limpia una palabra quitando signos de puntuación alrededor y convirtiendo a minúsculas.
    """
    signos_puntuacion = '.,;:¿?¡!"\'()[]{}'
    
    # Quitar signos al inicio
    while len(palabra) > 0 and palabra[0] in signos_puntuacion:
        palabra = palabra[1:]
    
    # Quitar signos al final
    while len(palabra) > 0 and palabra[-1] in signos_puntuacion:
        palabra = palabra[:-1]
    
    return palabra.lower()

def verificar_ortografia(archivo_usuario, diccionario):
    """
    Verifica la ortografía de un archivo y devuelve las palabras incorrectas encontradas.
    """
    palabras_incorrectas = []
    
    try:
        with open(archivo_usuario, 'r', encoding='utf-8') as archivo:
            for linea in archivo:
                palabras = linea.split()
                for palabra in palabras:
                    palabra_limpia = limpiar_palabra(palabra)
                    if palabra_limpia and palabra_limpia not in diccionario:
                        palabras_incorrectas.append(palabra_limpia)
    except FileNotFoundError:
        print(f"Error: No se pudo abrir el archivo {archivo_usuario}")
        return None
    
    return palabras_incorrectas

def main():
    # Cargar el diccionario de palabras correctas
    diccionario = cargar_diccionario("palabras.txt")
    if diccionario is None:
        return
    
    # Pedir al usuario el archivo a verificar
    nombre_archivo = input("Ingrese el nombre del archivo a verificar (ej. AlanTuring.txt): ")
    
    # Verificar ortografía
    errores = verificar_ortografia(nombre_archivo, diccionario)
    
    # Mostrar resultados
    if errores is not None:
        if len(errores) == 0:
            print("¡No se encontraron errores de ortografía!")
        else:
            print("\nPalabras con posible error ortográfico:")
            for palabra in errores:
                print(f"- {palabra}")
            print(f"\nTotal de errores encontrados: {len(errores)}")

if __name__ == "__main__":
    main()
