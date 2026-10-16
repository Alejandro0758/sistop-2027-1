Tarea: ejercicio de sincronización
Daniel Berumen Valdez

Problema: Santa Claus

Lenguaje: Python 3, solo con la biblioteca threading. Para correrlo:

    python3 santa.py

Se pueden cambiar NUM_ELFOS y NAVIDADES al inicio del archivo.

Estrategia:
Use semaforos. Cada reno y cada elfo es un hilo, y Santa corre en el hilo principal.

- mutex protege los contadores de renos y elfos.
- santa_sem es donde Santa se duerme. Lo despierta el ultimo reno en llegar o el tercer elfo con problema.
- renos_sem y reparto_sem hacen que los renos esperen a Santa y luego a que acabe el reparto.
- torniquete_elfos deja pasar solo a tres elfos a la vez. El tercero no lo suelta, y lo suelta el ultimo elfo del grupo cuando ya lo ayudaron.
- ayuda_sem y listos_sem: Santa ayuda a los tres y espera a que terminen antes de dormirse otra vez.

Si llegan los renos y hay elfos esperando, Santa atiende primero a los renos.
La solucion se basa en la de The Little Book of Semaphores (Downey).

Refinamientos: ninguno.

Dudas: los hilos de renos y elfos son daemon porque son ciclos infinitos, asi el programa termina cuando Santa completa sus navidades.

Por lo mismo, si al acabar la ultima navidad hay elfos esperando (por ejemplo ya iban 3/3 mientras Santa repartia), el programa termina y esos elfos se quedan sin ayuda. Pasa algo asi al final:

    🦌 Reno 5 regresa de vacaciones (9/9)
    🎅🛷 Santa: llegaron los renos, a repartir regalos (navidad 3)
    🧝 Elfo 7 tiene un problema (3/3)
    🎅🎁 Santa: regalos entregados
    🎄 Fin

Una forma de arreglarlo seria que Santa revise si quedo un grupo de elfos pendiente antes de terminar, o avisarle a los hilos que ya se acabo (con un Event) y hacer join, pero no lo implemente.
