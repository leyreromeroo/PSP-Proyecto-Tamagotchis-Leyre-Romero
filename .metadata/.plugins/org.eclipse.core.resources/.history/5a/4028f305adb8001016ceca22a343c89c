package com.leyre.romero.PSP_Proyecto_Tamagotchis;

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
	// Usamos 'volatile' para garantizar que todos los hilos vean el estado correcto.
	private volatile Estado estadoActual = Estado.OCIOSO;
	private String nombre;
	private long vida = System.currentTimeMillis(); 
	private final AtomicBoolean enEjecucion = new AtomicBoolean(true);


	public Tamagotchis(String nombre) {
		super();
		this.nombre = nombre;
	}

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	public long getVida() {
		return vida;
	}


	public void setVida(long vida) {
		this.vida = vida;
	}


	@Override
	public void run() {
		while(enEjecucion.get()) {
			if(getVida()<5) {
				System.out.println(nombre + " se ha muerto! Terminando el hilo.");
				solicitarParada(); 
				break;
			}else {

			}
		}
		System.out.println("Fin de " + nombre + ". El hilo ha terminado.");
	}

	public void solicitarParada() {
		enEjecucion.set(false);//Me creo una bandera para cada hilo
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

}


