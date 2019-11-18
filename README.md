
1. Para la solución, como primer paso realizaremos la creación de un volumen persistente que esté disponible para el cluster.

	1.1. Creamos un  GCE persistent disk}
	
			gcloud compute disks create --size=10GB --zone=us-east1-b gce-nfs-disk
			
	1.2 Configuramos el GCE persistent Disk
	
			gcloud container clusters get-credentials mappedinn-cluster --zone us-central1-a --project amine-testing
			
	1.3 Ejecutamos el deployment, volumen y service correspondiente al nfs-server para generar el pod, volumen y el servicio para poder ser accedido
		desde el servicio de php.
		
		
	NOTA: Para esta solución, solo será implementado el servicio de disco persistente para el servicio php, es posible realizarlo con mysql, pero
	dado los requerimientos, no fue necesario.
	
2. Ejecutamos el deployment, secret y luego el service correspondiente a mysql

3. Ejecutamos el deployment, configmap y luego el service correspondiente a php.

4. Luego de tener los servicios levantados, accedemos al pod de el servicio de nfs-server para generar el index.php persistente,

		kubectl exec -it <nombredelpod> -- /bin/bash
		cd exports
		vi index.php

5. Copiamos y pegamos lo siguiente 

		<?php
                // Create connection
                $conn = new mysqli('mysql',getenv("SYSTEM_DATASOURCE_USER"),getenv("MYSQL_ROOT_PASSWORD"));
                // Check connection
                if ($conn->connect_error) {
                die("Connection failed: " . $conn->connect_error);
                }
                echo "Connected successfully " ;
        ?>
		
6. Accedemos al puerto y observaremos Connected "Successfully"
