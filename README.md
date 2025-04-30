def cargar_diccionario(nombre_archivo):
    """Carga las palabras correctas desde un archivo a un diccionario."""
    try:
        with open(nombre_archivo, 'r') as archivo:  # Eliminado encoding
            palabras = {}
            for linea in archivo:
                palabra = linea.strip().lower()
                palabras[palabra] = 0
            return palabras
    except FileNotFoundError:
        print(f"Error: No se pudo abrir {nombre_archivo}")
        return None

def verificar_ortografia(archivo_usuario, diccionario):
    """Verifica la ortografía de un archivo."""
    palabras_incorrectas = []
    try:
        with open(archivo_usuario, 'r') as archivo:  # Eliminado encoding
            for linea in archivo:
                for palabra in linea.split():
                    palabra_limpia = limpiar_palabra(palabra)
                    if palabra_limpia and palabra_limpia not in diccionario:
                        palabras_incorrectas.append(palabra_limpia)
    except FileNotFoundError:
        print(f"Error: No se pudo abrir {archivo_usuario}")
        return None
    return palabras_incorrectas

# El resto del código (limpiar_palabra y main) se mantiene igual
