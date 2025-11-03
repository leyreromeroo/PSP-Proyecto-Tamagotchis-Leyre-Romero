package com.leyre.romero.PSP_Proyecto_Tamagotchis;

import java.time.Duration;
import java.util.Random;
import java.util.Scanner;
import java.util.concurrent.atomic.AtomicBoolean;

public class Tamagotchis implements Runnable{

	enum Estado{
		OCIOSO,
		JUGANDO,
		COMIENDO,
		LIMPIANDO,
		SUCIO,
		MUERTO
	}

	private static final long TIEMPO_MAXIMO_VIDA_MS = 5000;
	private static final long INTERVALO_SUCIEDAD_MS = 20000;
	private volatile Estado estadoActual = Estado.OCIOSO;
	private String nombre;
	private long vida = System.currentTimeMillis();
	private int suciedad = 0;
	private final AtomicBoolean enEjecucion = new AtomicBoolean(true);
	private long lastSuciedadTime;

	private final long horaNacimiento;

	public Tamagotchis(String nombre) {
		this.horaNacimiento = System.currentTimeMillis();
		this.nombre = nombre;
		this.lastSuciedadTime = this.horaNacimiento;
	}

	public String getNombre() {return nombre;}

	public void setNombre(String nombre) {this.nombre = nombre;}

	public long getVida() {return vida;}
	
	public int getSuciedad() { return suciedad; }

	public void setVida(long vida) {
		this.vida = vida;
	}

	public long getTiempoRestanteMS() {
		long tiempoTranscurrido = System.currentTimeMillis() - this.horaNacimiento;
		return TIEMPO_MAXIMO_VIDA_MS - tiempoTranscurrido;
	}

	// Método esencial que faltaba para la terminación cooperativa
	public void solicitarParada() {
		enEjecucion.set(false);
	}


	@Override
	public void run() {
		System.out.println(nombre + " ha nacido.");

		while(enEjecucion.get()) {
			long currentTime = System.currentTimeMillis();
            long tiempoTranscurridoVida = currentTime - horaNacimiento;
            long tiempoTranscurridoSuciedad = currentTime - lastSuciedadTime;

            // 1. --- LÓGICA DE MUERTE POR TIEMPO O POR SUCIEDAD ---
            if (tiempoTranscurridoVida >= TIEMPO_MAXIMO_VIDA_MS) {
                this.estadoActual = Estado.MUERTO;
                System.out.println("☠️ " + nombre + " se ha muerto de viejo! (" + (tiempoTranscurridoVida / 1000.0) + "s.)");
                solicitarParada();
                break;
            }
            
            if (this.suciedad >= 10) {
                this.estadoActual = Estado.MUERTO;
                System.out.println("☠️ " + nombre + " ha muerto por suciedad extrema! Nivel: " + suciedad);
                solicitarParada();
                break;
            }

            // 2. --- LÓGICA DE INCREMENTO DE SUCIEDAD (Cada 20 segundos) ---
            if (tiempoTranscurridoSuciedad >= INTERVALO_SUCIEDAD_MS) {
                this.suciedad++;
                this.lastSuciedadTime = currentTime; // Reinicia el contador de 20s
                
                System.out.println("🤢 " + nombre + " se ha ensuciado. Nivel: " + suciedad);
                
                if (this.suciedad >= 5 && this.suciedad < 10) {
                    System.out.println("⚠️ " + nombre + " está MUY SUCIO! Nivel: " + suciedad + ". Necesita un baño.");
                    this.estadoActual = Estado.SUCIO;
                } else if (this.suciedad < 5) {
                    this.estadoActual = Estado.OCIOSO; // Asegura que no está sucio
                }
            }
            
            // 3. --- PAUSA Y MANEJO DE INTERRUPCIONES ---
            // El hilo debe esperar un tiempo (ej. 100ms) para no sobrecargar el CPU
            try {
                // Si el tamagotchi está haciendo algo (comiendo/jugando/limpiando), espera que termine.
                if (estadoActual == Estado.OCIOSO || estadoActual == Estado.SUCIO) {
                     Thread.sleep(1000); // Pausa más larga si está ocioso
                } else {
                     Thread.sleep(100); // Pausa corta para revisar estado
                }
            } catch (InterruptedException e) {
                System.out.println(nombre + " fue interrumpido.");
                Thread.currentThread().interrupt();
                solicitarParada();
                break;
            }
        }
        System.out.println("Fin de la ejecución del hilo " + nombre);
		}


		public void comer() {
			estadoActual = Estado.COMIENDO;
			Random rnd = new Random();

			// Genera un tiempo de espera aleatorio entre 500 milisegundos y 2000 milisegundos (2 segundos)
			// El 'nextInt(1501)' da un número entre 0 y 1500. Al sumarle 500, da entre 500 y 2000.
			int tiempoComida = rnd.nextInt(1501) + 500; 

			System.out.println(nombre + " empieza a comer. Tardará: " + tiempoComida + "ms.");

			try {
				// Pausa la ejecución del hilo actual por el tiempo calculado
				Thread.sleep(tiempoComida);
			} catch (InterruptedException e) {
				// Se lanza si el hilo es interrumpido mientras está durmiendo
				System.out.println(nombre + " fue interrumpido mientras comía.");
				Thread.currentThread().interrupt(); // Restablece el estado de interrupción
			}

			System.out.println(nombre + " ha terminado de comer.");
			estadoActual = Estado.OCIOSO;
		}

		public void jugar() {
			estadoActual = Estado.JUGANDO;
			Scanner sc = new Scanner(System.in);
			Random rnd = new Random();

			System.out.println("ESTADO de " + nombre + ": " + estadoActual);
			System.out.println(nombre + " dice: ¡Ahora vamos a jugar! Tienes que calcular la suma.");


			int respuesta = -1; // Inicializamos la respuesta para que no coincida con 'suma' al inicio
			int suma = 0;       // Necesitamos la variable 'suma' fuera del bucle para la condición 'while'

			try {
				while (respuesta != suma) {

					int num1, num2 = 0;

					do {
						num1 = rnd.nextInt(10); 
						num2 = rnd.nextInt(10);
						suma = num1 + num2;
					} while (suma >= 10); // Repetir si la suma es 10 o más

					System.out.println("\n ¿Cuánto es " + num1 + " + " + num2 + "?");

					if (sc.hasNextInt()) {
						respuesta = sc.nextInt();

						if (respuesta == suma) {
							System.out.println(nombre + " dice: ¡Correcto! Eres muy inteligente.");
							break; // Salir del bucle 'while'
						} else {
							System.out.println(nombre + " dice: ¡Incorrecto! La respuesta correcta era " + suma + ". Inténtalo de nuevo.");
						}
					} else {
						System.out.println("Entrada inválida. Por favor, introduce un número.");
						sc.next(); // Consumir la entrada inválida para evitar un bucle infinito
					}
				}
			} catch (Exception e) {
				System.err.println(nombre + " ERROR durante el juego: " + e.getMessage());
			} finally {
				estadoActual = Estado.OCIOSO;
				System.out.println("ESTADO de " + nombre + ": " + estadoActual);
				sc.close();
			}
		}
		public void limpiar() {
			if (estadoActual != Estado.OCIOSO && estadoActual != Estado.SUCIO) {
	            System.out.println(nombre + " está ocupado (" + estadoActual + "), no puede limpiarse ahora.");
	            return;
	        }
	        
	        estadoActual = Estado.LIMPIANDO;
	        final long TIEMPO_BAÑO_MS = 5000; // 5 segundos

	        System.out.println(nombre + " comienza a limpiarse. [ESTADO: " + estadoActual + "]");
	        
	        try {
	            Thread.sleep(TIEMPO_BAÑO_MS);
	        } catch (InterruptedException e) {
	            System.out.println(nombre + " fue interrumpido durante el baño.");
	            Thread.currentThread().interrupt();
	        }

	        // Restablece el estado de suciedad y el estado general
	        this.suciedad = 0;
	        this.estadoActual = Estado.OCIOSO;
	        System.out.println(nombre + " está limpio (Suciedad: " + suciedad + "). [ESTADO: " + estadoActual + "]");
	    }
	}


