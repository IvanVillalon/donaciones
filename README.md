# donaciones
<?php 
function mostrarCarrito() {
    if (isset($_SESSION['carrito']) && !empty($_SESSION['carrito'])) {
        echo "<h2 class='text-center my-4'>Productos en el carrito</h2>";
        echo "<table class='table table-striped table-bordered'>";
        echo "<thead class='thead-dark'>
                <tr>
                    <th>Producto</th>
                    <th>Cantidad</th>
                    <th>Precio</th>
                    <th>Total</th>
                    <th>Acciones</th>
                </tr>
              </thead>";
        echo "<tbody>";

        $total = 0;

        // Recorrer el carrito
        foreach ($_SESSION['carrito'] as $id => $producto) {
            $subtotal = $producto['cantidad'] * $producto['precio'];
            $total += $subtotal;

            // Mostrar cada producto con campos para actualizar cantidad
            echo "<tr>";
            echo "<td>{$producto['nombre']}</td>";
            echo "<td>
                    <form id='form-{$producto['id']}' method='post' action='modificar_carrito.php'>
                        <input type='hidden' name='id_producto' value='{$producto['id']}'>
                        <input type='number' name='cantidad' value='{$producto['cantidad']}' min='1' class='form-control' style='width: 80px; display: inline;'>
                        <button type='submit' class='btn btn-primary btn-sm'>Actualizar</button>
                    </form>
                  </td>";
            echo "<td>{$producto['precio']} $</td>";
            echo "<td>{$subtotal} $</td>";
            
            echo "<td>
                    <form method='post' action='modificar_carrito.php' style='display:inline;'>
                        <input type='hidden' name='id_producto' value='{$producto['id']}'>
                        <input type='hidden' name='accion' value='eliminar'>
                        <button type='submit' class='btn btn-danger btn-sm'>Eliminar</button>
                    </form>
                  </td>";
            echo "</tr>";
        }

        echo "</tbody>";
        echo "</table>";
        echo "<h4 class='text-right'>Total: <strong>{$total} $</strong></h4>";

        // Botón "Realizar Compra" fuera del bucle
        echo "<form method='post' action='realizar_compra.php' style='display:inline;'>";
        // Agregar un campo oculto para cada producto en el carrito
        foreach ($_SESSION['carrito'] as $id => $producto) {
            echo "<input type='hidden' name='productos[{$id}]' value='" . json_encode($producto) . "'>";
        }
        echo "<button type='submit' class='btn btn-danger btn-sm'>Realizar compra</button>";
        echo "</form>";

    } else {
        echo "<p class='text-center'>Tu carrito está vacío.</p>";
    }
}

?>
