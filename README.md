# actividad-final
indice de masa corporal
package controlador;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JOptionPane;
import modelo.Consultas;
import modelo.pasientes;
import vista.formulario;

public class ControlDatos implements ActionListener {

    private Consultas modC;
    private pasientes modp;
    private formulario visf;

    public ControlDatos(Consultas modC, pasientes modp, formulario visf) {
        this.modC = modC;
        this.modp = modp;
        this.visf = visf;
        this.visf.btnGuardar.addActionListener(this);
        this.visf.btnModificar.addActionListener(this);
        this.visf.btnEliminar.addActionListener(this);
        this.visf.btnLimpiar.addActionListener(this);
        this.visf.btnBuscar.addActionListener(this);
    }

    public void iniciar() {
        visf.setTitle("pasientes");
        visf.setLocationRelativeTo(null);
        visf.txtid.setVisible(false);
    }

    @Override
    public void actionPerformed(ActionEvent e) {

        if (e.getSource() == visf.btnGuardar) {
            modp.setNombre(visf.txtNombre.getText());
            modp.setApellido(visf.txtApellido.getText());
            modp.setEstatura(Double.parseDouble(visf.txtestatura.getText()));
            modp.setPeso(Double.parseDouble(visf.txtpeso.getText()));
            modp.setEdad(Integer.parseInt(visf.txtedad.getText()));
            modp.setSexo(visf.txtsexo.getText());
            modp.setImc(Double.parseDouble(visf.txtimc.getText()));
            modp.setFecha(Double.parseDouble(visf.txtfecha.getText()));

            if (modC.registrar(modp)) {
                JOptionPane.showMessageDialog(null, "Registro exiroso");
                limpiar();
            } else {
                JOptionPane.showMessageDialog(null, "Erros verifique datos");
                limpiar();

            }
        }
        if (e.getSource() == visf.btnModificar) {
            modp.setId(Integer.parseInt(visf.txtid.getText()));
            modp.setNombre(visf.txtNombre.getText());
            modp.setApellido(visf.txtApellido.getText());
            modp.setEstatura(Double.parseDouble(visf.txtestatura.getText()));
            modp.setPeso(Double.parseDouble(visf.txtpeso.getText()));
            modp.setEdad(Integer.parseInt(visf.txtedad.getText()));
            modp.setSexo(visf.txtsexo.getText());
            modp.setImc(Double.parseDouble(visf.txtimc.getText()));
            modp.setFecha(Double.parseDouble(visf.txtfecha.getText()));

            if (modC.modificar(modp)) {
                JOptionPane.showMessageDialog(null, "Registro modificado");
                limpiar();
            } else {
                JOptionPane.showMessageDialog(null, "Erros al modificar verifique datos");
                limpiar();
            }
        }
        
                if (e.getSource() == visf.btnEliminar) {
            modp.setId(Integer.parseInt(visf.txtid.getText()));
            
            if (modC.eliminar(modp)) {
                JOptionPane.showMessageDialog(null, "Registro eliminado");
                limpiar();
            } else {
                JOptionPane.showMessageDialog(null, "Erros al eliminar");
                limpiar();
            }
        }
        
        
       if (e.getSource() == visf.btnBuscar) {
            modp.setNombre(visf.txtNombre.getText());
            
            if (modC.buscar(modp)) 
            {
                visf.txtid.setText(String.valueOf(modp.getId()));
                visf.txtNombre.setText(modp.getNombre());
                visf.txtApellido.setText(modp.getApellido());
                visf.txtestatura.setText(String.valueOf(modp.getEstatura()));
                visf.txtpeso.setText(String.valueOf(modp.getPeso()));
                visf.txtedad.setText(String.valueOf(modp.getEdad()));
                visf.txtsexo.setText(modp.getSexo());
                visf.txtimc.setText(String.valueOf(modp.getImc()));
                visf.txtfecha.setText(String.valueOf(modp.getFecha()));
                
                }
            else 
                {
                JOptionPane.showMessageDialog(null, "No se encontraron registros");
                limpiar();
            }
        }
       
       
       if (e.getSource() == visf.btnLimpiar) {
                limpiar();
            }
        }         

    private void limpiar() {
        {

        visf.txtNombre.setText(null);
        visf.txtApellido.setText(null);
        visf.txtestatura.setText(null);
        visf.txtpeso.setText(null);
        visf.txtedad.setText(null);
        visf.txtsexo.setText(null);
        visf.txtimc.setText(null);
        visf.txtfecha.setText(null);

    }
}
}




package evidenciafinal;

import controlador.ControlDatos;
import modelo.Consultas;
import modelo.pasientes;
import vista.formulario;

public class EvidenciaFinal {

  
    public static void main(String[] args) {
        
        Consultas modC = new Consultas();
        pasientes modp = new pasientes();
        formulario visf = new formulario();
        
        ControlDatos cd = new ControlDatos(modC, modp, visf);
        
        cd.iniciar();
        visf.setVisible(true);
        
        
    }
    
}




package modelo;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.logging.Level;
import java.util.logging.Logger;



public class Conexion {
    
    private final String base       = "imc_mvc";
    private final String user       = "root";
    private final String password   = "1234";
    private final String url        = "jbdc:mysql://localhost:3306/" + base ;
    private Connection con = null;
    
    public Connection getConexion ()
            
    {
        try {
            
            Class.forName("com.mysql.cj.jdbc.Driver");
            con = (Connection)DriverManager.getConnection(this.url, this.user, this.password );
        
        }catch(SQLException e)
        {
            System.err.println(e);
        } catch (ClassNotFoundException ex) {
            Logger.getLogger(Conexion.class.getName()).log(Level.SEVERE, null, ex);
        }
        return con;
    }
    
    
}


ackage modelo;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Consultas extends Conexion {

public boolean registrar(pasientes pas)  {
        PreparedStatement ps = null;
        Connection con = getConexion();
        String sql = "INSER INTO pasientes (Nombre, Apellido,estatura, peso, edad,sexo,imc, fecha) VALUES (?,?,?,?,?,?,?)";

        try {

            ps = con.prepareStatement(sql);
            ps.setString(1, pas.getNombre());
            ps.setString(2, pas.getApellido());
            ps.setDouble(3, pas.getEstatura());
            ps.setDouble(4, pas.getPeso());
            ps.setInt(5, pas.getEdad());
            ps.setString(6, pas.getSexo());
            ps.setDouble(7, pas.getImc());
            ps.setDouble(8, pas.getFecha());
            ps.execute();
            return true;
        } catch (SQLException e) {
            System.err.println(e);
            return false;
        } finally {
            try {
                con.close();
            } catch (SQLException e) {
                
                    System.err.println(e);
            }
        }
        
        
    }
    
public boolean modificar(pasientes pas)  {
        PreparedStatement ps = null;
        Connection con = getConexion();
        String sql = "UPDATE  pasientes SET Nombre=?, Apellido=?,estatura=?, peso=?, edad=?,sexo=?,imc=?, fecha=?, WHERE id=?";
        
        try {

            ps = con.prepareStatement(sql);
            ps.setString(1, pas.getNombre());
            ps.setString(2, pas.getApellido());
            ps.setDouble(3, pas.getEstatura());
            ps.setDouble(4, pas.getPeso());
            ps.setInt(5, pas.getEdad());
            ps.setString(6, pas.getSexo());
            ps.setDouble(7, pas.getImc());
            ps.setDouble(8, pas.getFecha());
            ps.setInt(9, pas.getId());
            ps.execute();
            return true;
        } catch (SQLException e) {
            System.err.println(e);
            return false;
        } finally {
            try {
                con.close();
            } catch (SQLException e) {
                
                    System.err.println(e);
            }
        }
    }

    
public boolean eliminar(pasientes pas)  {
        PreparedStatement ps = null;
        Connection con = getConexion();
        String sql = "DELETE FROM pasientes  WHERE id=?";
        
        try {

            ps = con.prepareStatement(sql);
            ps.setInt(1, pas.getId());
            ps.execute();
            return true;
        } catch (SQLException e) {
            System.err.println(e);
            return false;
        } finally {
            try {
                con.close();
            } catch (SQLException e) {
                
                    System.err.println(e);
            }
        }
    }




package modelo;


public class pasientes {
    
    private int id;
    private String Nombre;
    private String Apellido;
    private Double estatura;
    private Double peso;
    private int edad;
    private String sexo;
    private Double imc;    
    private Double fecha;    

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getNombre() {
        return Nombre;
    }

    public void setNombre(String Nombre) {
        this.Nombre = Nombre;
    }

    public String getApellido() {
        return Apellido;
    }

    public void setApellido(String Apellido) {
        this.Apellido = Apellido;
    }

    public Double getEstatura() {
        return estatura;
    }

    public void setEstatura(Double estatura) {
        this.estatura = estatura;
    }

    public Double getPeso() {
        return peso;
    }

    public void setPeso(Double peso) {
        this.peso = peso;
    }

    public int getEdad() {
        return edad;
    }

    public void setEdad(int edad) {
        this.edad = edad;
    }

    public String getSexo() {
        return sexo;
    }

    public void setSexo(String sexo) {
        this.sexo = sexo;
    }

    public Double getImc() {
        return imc;
    }

    public void setImc(Double imc) {
        this.imc = imc;
    }

    public Double getFecha() {
        return fecha;
    }

    public void setFecha(Double fecha) {
        this.fecha = fecha;
    }
        
        

}




*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/GUIForms/JFrame.java to edit this template
 */
package vista;

/**
 *
 * @author pdenn
 */
public class formulario extends javax.swing.JFrame {

    /**
     * Creates new form formulario
     */
    public formulario() {
        initComponents();
    }

    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        jScrollPane1 = new javax.swing.JScrollPane();
        jList1 = new javax.swing.JList<>();
        jLabel1 = new javax.swing.JLabel();
        jLabel2 = new javax.swing.JLabel();
        jLabel3 = new javax.swing.JLabel();
        jLabel4 = new javax.swing.JLabel();
        jLabel5 = new javax.swing.JLabel();
        jLabel6 = new javax.swing.JLabel();
        jLabel7 = new javax.swing.JLabel();
        jLabel8 = new javax.swing.JLabel();
        jLabel9 = new javax.swing.JLabel();
        txtid = new javax.swing.JTextField();
        txtNombre = new javax.swing.JTextField();
        txtApellido = new javax.swing.JTextField();
        txtestatura = new javax.swing.JTextField();
        txtpeso = new javax.swing.JTextField();
        txtedad = new javax.swing.JTextField();
        txtimc = new javax.swing.JTextField();
        txtfecha = new javax.swing.JTextField();
        txtsexo = new javax.swing.JTextField();
        btnGuardar = new javax.swing.JButton();
        btnModificar = new javax.swing.JButton();
        btnEliminar = new javax.swing.JButton();
        btnBuscar = new javax.swing.JButton();
        btnLimpiar = new javax.swing.JButton();

        jList1.setModel(new javax.swing.AbstractListModel<String>() {
            String[] strings = { "Item 1", "Item 2", "Item 3", "Item 4", "Item 5" };
            public int getSize() { return strings.length; }
            public String getElementAt(int i) { return strings[i]; }
        });
        jScrollPane1.setViewportView(jList1);

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        jLabel1.setText("id");

        jLabel2.setText("Nombre");

        jLabel3.setText("Apellido");

        jLabel4.setText("estatura");

        jLabel5.setText("peso");

        jLabel6.setText("edad");

        jLabel7.setText("sexo");

        jLabel8.setText("imc");

        jLabel9.setText("fecha");

        txtid.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                txtidActionPerformed(evt);
            }
        });

        txtestatura.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                txtestaturaActionPerformed(evt);
            }
        });

        txtpeso.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                txtpesoActionPerformed(evt);
            }
        });

        txtedad.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                txtedadActionPerformed(evt);
            }
        });

        txtimc.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                txtimcActionPerformed(evt);
            }
        });

        txtfecha.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                txtfechaActionPerformed(evt);
            }
        });

        txtsexo.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                txtsexoActionPerformed(evt);
            }
        });

        btnGuardar.setText("Guardar");

        btnModificar.setText("Modificar");

        btnEliminar.setText("Eliminar");

        btnBuscar.setText("Buscar");

        btnLimpiar.setText("Limpiar");

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addGroup(layout.createSequentialGroup()
                        .addGap(33, 33, 33)
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                            .addComponent(jLabel4)
                            .addComponent(jLabel3)
                            .addComponent(jLabel2)
                            .addComponent(jLabel1)
                            .addComponent(jLabel5)
                            .addComponent(jLabel6)
                            .addComponent(jLabel7)
                            .addComponent(jLabel8)
                            .addComponent(jLabel9))
                        .addGap(31, 31, 31)
                        .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                            .addGroup(layout.createSequentialGroup()
                                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.TRAILING, false)
                                    .addComponent(txtpeso, javax.swing.GroupLayout.Alignment.LEADING)
                                    .addComponent(txtid, javax.swing.GroupLayout.Alignment.LEADING, javax.swing.GroupLayout.PREFERRED_SIZE, 44, javax.swing.GroupLayout.PREFERRED_SIZE)
                                    .addComponent(txtNombre, javax.swing.GroupLayout.Alignment.LEADING, javax.swing.GroupLayout.DEFAULT_SIZE, 120, Short.MAX_VALUE)
                                    .addComponent(txtApellido, javax.swing.GroupLayout.Alignment.LEADING)
                                    .addComponent(txtestatura, javax.swing.GroupLayout.Alignment.LEADING))
                                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED, javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
                                .addComponent(btnBuscar, javax.swing.GroupLayout.PREFERRED_SIZE, 75, javax.swing.GroupLayout.PREFERRED_SIZE))
                            .addGroup(layout.createSequentialGroup()
                                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING, false)
                                    .addComponent(txtsexo, javax.swing.GroupLayout.DEFAULT_SIZE, 118, Short.MAX_VALUE)
                                    .addComponent(txtimc)
                                    .addComponent(txtfecha))
                                .addGap(0, 0, Short.MAX_VALUE))))
                    .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                        .addContainerGap(javax.swing.GroupLayout.DEFAULT_SIZE, Short.MAX_VALUE)
                        .addComponent(btnLimpiar, javax.swing.GroupLayout.PREFERRED_SIZE, 75, javax.swing.GroupLayout.PREFERRED_SIZE)))
                .addGap(23, 23, 23))
            .addGroup(layout.createSequentialGroup()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.TRAILING)
                    .addGroup(layout.createSequentialGroup()
                        .addContainerGap()
                        .addComponent(txtedad, javax.swing.GroupLayout.PREFERRED_SIZE, 118, javax.swing.GroupLayout.PREFERRED_SIZE))
                    .addGroup(javax.swing.GroupLayout.Alignment.LEADING, layout.createSequentialGroup()
                        .addGap(19, 19, 19)
                        .addComponent(btnGuardar)
                        .addGap(40, 40, 40)
                        .addComponent(btnModificar, javax.swing.GroupLayout.PREFERRED_SIZE, 92, javax.swing.GroupLayout.PREFERRED_SIZE)))
                .addGap(18, 18, 18)
                .addComponent(btnEliminar, javax.swing.GroupLayout.PREFERRED_SIZE, 86, javax.swing.GroupLayout.PREFERRED_SIZE)
                .addGap(0, 53, Short.MAX_VALUE))
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addContainerGap()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(jLabel1)
                    .addComponent(txtid, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addGap(18, 18, 18)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(jLabel2)
                    .addComponent(txtNombre, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE)
                    .addComponent(btnBuscar))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(jLabel3)
                    .addComponent(txtApellido, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addComponent(btnLimpiar)
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(jLabel4)
                    .addComponent(txtestatura, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(jLabel5)
                    .addComponent(txtpeso, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(jLabel6)
                    .addComponent(txtedad, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
                    .addComponent(jLabel7)
                    .addComponent(txtsexo, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addGap(18, 18, 18)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(jLabel8)
                    .addComponent(txtimc, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(jLabel9)
                    .addComponent(txtfecha, javax.swing.GroupLayout.PREFERRED_SIZE, javax.swing.GroupLayout.DEFAULT_SIZE, javax.swing.GroupLayout.PREFERRED_SIZE))
                .addGap(18, 18, Short.MAX_VALUE)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.BASELINE)
                    .addComponent(btnGuardar)
                    .addComponent(btnModificar)
                    .addComponent(btnEliminar))
                .addContainerGap())
        );

        pack();
    }// </editor-fold>                        

    private void txtidActionPerformed(java.awt.event.ActionEvent evt) {                                      
        // TODO add your handling code here:
    }                                     

    private void txtestaturaActionPerformed(java.awt.event.ActionEvent evt) {                                            
        // TODO add your handling code here:
    }                                           

    private void txtpesoActionPerformed(java.awt.event.ActionEvent evt) {                                        
        // TODO add your handling code here:
    }                                       

    private void txtedadActionPerformed(java.awt.event.ActionEvent evt) {                                        
        // TODO add your handling code here:
    }                                       

    private void txtimcActionPerformed(java.awt.event.ActionEvent evt) {                                       
        // TODO add your handling code here:
    }                                      

    private void txtfechaActionPerformed(java.awt.event.ActionEvent evt) {                                         
        // TODO add your handling code here:
    }                                        

    private void txtsexoActionPerformed(java.awt.event.ActionEvent evt) {                                        
        // TODO add your handling code here:
    }                                       

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html 
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(formulario.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(formulario.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(formulario.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(formulario.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(() -> {
            new formulario().setVisible(true);
        });
    }

    // Variables declaration - do not modify                     
    public javax.swing.JButton btnBuscar;
    public javax.swing.JButton btnEliminar;
    public javax.swing.JButton btnGuardar;
    public javax.swing.JButton btnLimpiar;
    public javax.swing.JButton btnModificar;
    private javax.swing.JLabel jLabel1;
    private javax.swing.JLabel jLabel2;
    private javax.swing.JLabel jLabel3;
    private javax.swing.JLabel jLabel4;
    private javax.swing.JLabel jLabel5;
    private javax.swing.JLabel jLabel6;
    private javax.swing.JLabel jLabel7;
    private javax.swing.JLabel jLabel8;
    private javax.swing.JLabel jLabel9;
    private javax.swing.JList<String> jList1;
    private javax.swing.JScrollPane jScrollPane1;
    public javax.swing.JTextField txtApellido;
    public javax.swing.JTextField txtNombre;
    public javax.swing.JTextField txtedad;
    public javax.swing.JTextField txtestatura;
    public javax.swing.JTextField txtfecha;
    public javax.swing.JTextField txtid;
    public javax.swing.JTextField txtimc;
    public javax.swing.JTextField txtpeso;
    public javax.swing.JTextField txtsexo;
    // End of variables declaration                   
}


