# CONVERTIDOR de Unidades de Almacenamiento

Este proyecto en **Java Swing** permite convertir valores entre distintas unidades de almacenamiento: **bits, bytes, kilobytes, megabytes, gigabytes y terabytes**. Incluye conversión automática al cambiar unidades o al ingresar un valor, y un botón para limpiar los campos.


# Convertidor de Unidades de Almacenamiento

Este proyecto está diseñado para convertir valores de almacenamiento digital entre distintas unidades: **Bits, Bytes, Kilobytes, Megabytes, Gigabytes, Terabytes y Petabytes**. Integra una interfaz gráfica en **Java Swing** y permite conversiones precisas usando base **1000**.

---

## 📁 Estructura del Proyecto

---

## ▣══ Paquete: `Interfaz` ══▣

Contiene las ventanas gráficas y la interacción con el usuario.

### ┣▇ `Frame1.java`
Ventana principal del convertidor, con campos de entrada, selección de unidades y botones para convertir o limpiar los datos.

#### Componentes principales:
- `JTextField txtValor`: Campo para ingresar el valor a convertir.
- `Choice chUnidadOrigen`: Combo para seleccionar la unidad de origen.
- `Choice chUnidadDestino`: Combo para seleccionar la unidad destino.
- `JTextField txtResultado`: Campo donde se muestra el resultado de la conversión.
- `JButton btnConvertir`: Botón que ejecuta la conversión.
- `JButton btnLimpiar`: Botón que limpia todos los campos y selecciones.

#### Métodos principales:
- `llenarChoices()`: Llena los combos de unidades con opciones: Bits, Bytes, KB, MB, GB, TB, PB.
- `agregarListeners()`: Añade los eventos de conversión y limpieza.
- `convertirABYtes(double valor, String unidad)`: Convierte cualquier unidad a **Bytes** usando base 1000.
- `convertirDesdeBytes(double bytes, String unidadDestino)`: Convierte desde Bytes hacia la unidad seleccionada.
- `limpiarCampos()`: Limpia todos los JTextFields y resetea los combos a su valor por defecto.

---

## ▣══ Paquete: `Utilidades` ══▣

Contiene funciones auxiliares para los cálculos y formato de valores.

### ┣▇ `ConversionUtil.java`
Clase con métodos estáticos que realizan las conversiones entre unidades de almacenamiento.

#### Métodos:
- `public static double convertir(double valor, String unidadOrigen, String unidadDestino)`
  - Convierte un valor de `unidadOrigen` a `unidadDestino`.
  - Internamente primero convierte a Bytes y luego a la unidad final.
  - Ejemplo:
    ```java
    double resultado = ConversionUtil.convertir(1000, "MB", "GB"); // Devuelve 1.0
    ```

- `public static String formatear(double valor)`
  - Da formato a un número con hasta 10 decimales y separadores de miles.

---

## ▣══ Lógica de Conversión

### Conversión base 1000:

| Unidad | Equivalencia en Bytes |
|--------|---------------------|
| Bit    | 1/8 Byte            |
| Byte   | 1 Byte              |
| KB     | 1000 Bytes          |
| MB     | 1000 KB             |
| GB     | 1000 MB             |
| TB     | 1000 GB             |
| PB     | 1000 TB             |

#### Ejemplo de flujo de conversión:
1. Usuario ingresa `2048` en el campo `txtValor`.
2. Selecciona `MB` como unidad de origen y `GB` como unidad destino.
3. Al presionar `btnConvertir`:
   - `convertirABYtes(2048, "MB")` → 2048 × 1,000,000 = 2,048,000,000 Bytes.
   - `convertirDesdeBytes(2,048,000,000, "GB")` → 2,048,000,000 ÷ 1,000,000,000 = 2.048 GB.
4. Resultado mostrado en `txtResultado`: `2.048 GB`.

---

## ▣══ Eventos de Interfaz

### `btnConvertirActionPerformed`
- Obtiene valor del usuario.
- Valida que no esté vacío ni sea negativo.
- Llama a `ConversionUtil.convertir(valor, unidadOrigen, unidadDestino)`.
- Muestra el resultado en `txtResultado`.

### `btnLimpiarActionPerformed`
- Limpia `txtValor` y `txtResultado`.
- Resetea `chUnidadOrigen` y `chUnidadDestino` al primer valor.

---

### ▣ Ejemplo de inicialización

```java
public static void main(String[] args) {
    java.awt.EventQueue.invokeLater(() -> {
        new Frame1().setVisible(true);
    });
}




```java
package Interfaz;

import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.text.DecimalFormat;
import javax.swing.JOptionPane;

/**
 * Clase principal Frame1
 * Implementa la interfaz gráfica y la lógica de conversión de unidades de almacenamiento.
 */
public class Frame1 extends javax.swing.JFrame {

    // Formato decimal para mostrar resultados
    private DecimalFormat df = new DecimalFormat("#,##0.##########");

    /**
     * Constructor
     * Inicializa la interfaz, llena los Choice y agrega los listeners para conversión automática
     */
    public Frame1() {
        initComponents();      
        llenarChoices();
        agregarListeners();
    }

    /**
     * Llena los Choice con las unidades de almacenamiento
     */
    private void llenarChoices() {
        choice1.removeAll();
        choice2.removeAll();
        String[] unidades = {"bits", "bytes", "kilobytes", "megabytes", "gigabytes", "terabytes"};
        for (String unidad : unidades) {
            choice1.add(unidad);
            choice2.add(unidad);
        }
    }

    /**
     * Agrega listeners a los Choice y TextField para conversión automática
     */
    private void agregarListeners() {
        // Listener para unidad de origen
        choice1.addItemListener(e -> {
            if (e.getStateChange() == ItemEvent.SELECTED) {
                convertirAutomaticamente();
            }
        });

        // Listener para unidad de destino
        choice2.addItemListener(e -> {
            if (e.getStateChange() == ItemEvent.SELECTED) {
                convertirAutomaticamente();
            }
        });

        // Listener para valor de entrada
        textField3.addKeyListener(new java.awt.event.KeyAdapter() {
            public void keyReleased(java.awt.event.KeyEvent evt) {
                convertirAutomaticamente();
            }
        });
    }

    /**
     * Convierte automáticamente si el campo de entrada no está vacío
     */
    private void convertirAutomaticamente() {
        if (!textField3.getText().trim().isEmpty()) {
            convertirUnidades();
        }
    }

    /**
     * Inicializa los componentes de la interfaz gráfica
     */
    @SuppressWarnings("unchecked")
    private void initComponents() {

        jLabel1 = new javax.swing.JLabel();
        choice1 = new java.awt.Choice();
        choice2 = new java.awt.Choice();
        textField2 = new java.awt.TextField();
        textField3 = new java.awt.TextField();
        jLabel2 = new javax.swing.JLabel();
        jButton1 = new javax.swing.JButton();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        jLabel1.setFont(new java.awt.Font("Segoe UI", 0, 24));
        jLabel1.setText("Trasnformador ");

        textField2.addActionListener(evt -> textField2ActionPerformed(evt));
        textField3.addActionListener(evt -> textField3ActionPerformed(evt));

        jLabel2.setFont(new java.awt.Font("Segoe UI", 0, 36));
        jLabel2.setText("=");

        jButton1.setText("Limpiar");
        jButton1.addActionListener(evt -> jButton1ActionPerformed(evt));

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(39, 39, 39)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(jLabel1)
                        .addContainerGap(javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE))
                    .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.TRAILING)
                            .addGroup(layout.createSequentialGroup()
                                .addGap(0, 0, Short.MAX_VALUE)
                                .addComponent(jButton1))
                            .addGroup(layout.createSequentialGroup()
                                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                                    .addComponent(choice1, javax.swing.GroupLayout.PREFERRED_SIZE, 98, javax.swing.GroupLayout.PREFERRED_SIZE)
                                    .addComponent(textField3, javax.swing.GroupLayout.PREFERRED_SIZE, 94, javax.swing.GroupLayout.PREFERRED_SIZE))
                                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 34, Short.MAX_VALUE)
                                .addComponent(jLabel2)))
                        .addGap(29, 29, 29)
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.TRAILING)
                            .addComponent(choice2, javax.swing.GroupLayout.PREFERRED_SIZE, 94, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addComponent(textField2, javax.swing.GroupLayout.PREFERRED_SIZE, 94, javax.swing.GroupLayout.PREFERRED_SIZE))
                        .addGap(81, 81, 81))))
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGap(21, 21, 21)
                .addComponent(jLabel1)
                .addGap(58, 58, 58)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.TRAILING)
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(textField3, javax.swing.GroupLayout.PREFERRED_SIZE, 37, javax.swing.GroupLayout.PREFERRED_SIZE)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addComponent(choice1, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                    .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                        .addComponent(jLabel2)
                        .addGroup(layout.createSequentialGroup()
                            .addComponent(textField2, javax.swing.GroupLayout.PREFERRED_SIZE, 37, javax.swing.GroupLayout.PREFERRED_SIZE)
                            .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                            .addComponent(choice2, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, 51, Short.MAX_VALUE)
                .addComponent(jButton1)
                .addGap(48, 48, 48))
        );

        pack();
    }

    /**
     * Acción al presionar Enter en el campo de entrada
     */
    private void textField3ActionPerformed(java.awt.event.ActionEvent evt) {                                           
        convertirUnidades();
    }                                          

    /**
     * Acción en textField2 (vacía, puede usarse si se desea)
     */
    private void textField2ActionPerformed(java.awt.event.ActionEvent evt) {                                           
        // No implementado
    }                                          

    /**
     * Botón Limpiar: limpia los campos de entrada y selección
     */
    private void jButton1ActionPerformed(java.awt.event.ActionEvent evt) {                                         
        limpiarCampos(); 
    }                                        

    /**
     * Convierte el valor ingresado de la unidad origen a la unidad destino
     */
    private void convertirUnidades() {
        try {
            String unidadOrigen = choice1.getSelectedItem();
            String unidadDestino = choice2.getSelectedItem();
            double valor = Double.parseDouble(textField3.getText());

            double valorEnBytes = convertirABytes(valor, unidadOrigen);
            double resultado = convertirDesdeBytes(valorEnBytes, unidadDestino);

            textField2.setText(df.format(resultado));

        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this, "Por favor ingrese un número válido", "Error", JOptionPane.ERROR_MESSAGE);
        } catch (Exception ex) {
            JOptionPane.showMessageDialog(this, "Error en la conversión: " + ex.getMessage(), "Error", JOptionPane.ERROR_MESSAGE);
        }
    }

    /**
     * Convierte cualquier unidad a bytes
     */
    private double convertirABytes(double valor, String unidad) {
        switch (unidad) {
            case "bits": return valor / 8.0;
            case "bytes": return valor;
            case "kilobytes": return valor * 1000;
            case "megabytes": return valor * 1000 * 1000;
            case "gigabytes": return valor * 1000 * 1000 * 1000;
            case "terabytes": return valor * 1000L * 1000L * 1000L * 1000L;
            default: return valor;
        }
    }

    /**
     * Convierte desde bytes a la unidad destino
     */
    private double convertirDesdeBytes(double valorEnBytes, String unidad) {
        switch (unidad) {
            case "bits": return valorEnBytes * 8.0;
            case "bytes": return valorEnBytes;
            case "kilobytes": return valorEnBytes / 1000;
            case "megabytes": return valorEnBytes / (1000 * 1000);
            case "gigabytes": return valorEnBytes / (1000 * 1000 * 1000);
            case "terabytes": return valorEnBytes / (1000L * 1000L * 1000L * 1000L);
            default: return valorEnBytes;
        }
    }

    /**
     * Limpia los campos de entrada y selección
     */
    private void limpiarCampos() {
        textField3.setText("");
        textField2.setText("");
        choice1.select(0);
        choice2.select(0);
        textField3.requestFocus();
    }

    /**
     * Método principal: ejecuta la aplicación
     */
    public static void main(String args[]) {
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (Exception ex) {
            java.util.logging.Logger.getLogger(Frame1.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }

        java.awt.EventQueue.invokeLater(() -> new Frame1().setVisible(true));
    }

    // Variables de la interfaz
    private java.awt.Choice choice1;
    private java.awt.Choice choice2;
    private javax.swing.JButton jButton1;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JLabel jLabel2;
    private java.awt.TextField textField2;
    private java.awt.TextField textField3;
}

