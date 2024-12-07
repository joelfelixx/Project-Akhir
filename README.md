import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

public class PemeringkatanMahasiswaGUI {

    static class Mahasiswa {
        String nama;
        double ipk;

        public Mahasiswa(String nama, double ipk) {
            this.nama = nama;
            this.ipk = ipk;
        }
    }

    public static void quickSort(ArrayList<Mahasiswa> list, int low, int high) {
        if (low < high) {
            int pi = partition(list, low, high);
            quickSort(list, low, pi - 1);
            quickSort(list, pi + 1, high);
        }
    }

    private static int partition(ArrayList<Mahasiswa> list, int low, int high) {
        double pivot = list.get(high).ipk;
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (list.get(j).ipk > pivot) {
                i++;
                Mahasiswa temp = list.get(i);
                list.set(i, list.get(j));
                list.set(j, temp);
            }
        }
        Mahasiswa temp = list.get(i + 1);
        list.set(i + 1, list.get(high));
        list.set(high, temp);
        return i + 1;
    }

    public static void main(String[] args) {
        ArrayList<Mahasiswa> mahasiswaList = new ArrayList<>();

        JFrame frame = new JFrame("Aplikasi Pemeringkatan Mahasiswa");
        frame.setSize(600, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel inputPanel = new JPanel(new GridLayout(3, 2, 10, 10));
        inputPanel.setBorder(BorderFactory.createTitledBorder("Input Data Mahasiswa"));

        JLabel nameLabel = new JLabel("Nama Mahasiswa:");
        JTextField nameField = new JTextField();
        JLabel ipkLabel = new JLabel("IPK Mahasiswa:");
        JTextField ipkField = new JTextField();
        JButton addButton = new JButton("Tambahkan");

        inputPanel.add(nameLabel);
        inputPanel.add(nameField);
        inputPanel.add(ipkLabel);
        inputPanel.add(ipkField);
        inputPanel.add(addButton);

        JPanel tablePanel = new JPanel();
        tablePanel.setBorder(BorderFactory.createTitledBorder("Peringkat Mahasiswa"));

        String[] columnNames = {"Peringkat", "Nama", "IPK"};
        DefaultTableModel tableModel = new DefaultTableModel(columnNames, 0);
        JTable table = new JTable(tableModel);
        JScrollPane scrollPane = new JScrollPane(table);
        tablePanel.add(scrollPane);

        JPanel buttonPanel = new JPanel();
        JButton sortButton = new JButton("Urutkan");
        buttonPanel.add(sortButton);

        frame.setLayout(new BorderLayout());
        frame.add(inputPanel, BorderLayout.NORTH);
        frame.add(tablePanel, BorderLayout.CENTER);
        frame.add(buttonPanel, BorderLayout.SOUTH);

        addButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String nama = nameField.getText();
                double ipk;
                try {
                    ipk = Double.parseDouble(ipkField.getText());
                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(frame, "IPK harus berupa angka!", "Error", JOptionPane.ERROR_MESSAGE);
                    return;
                }
                mahasiswaList.add(new Mahasiswa(nama, ipk));
                nameField.setText("");
                ipkField.setText("");
                JOptionPane.showMessageDialog(frame, "Data berhasil ditambahkan!");
            }
        });

        sortButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                quickSort(mahasiswaList, 0, mahasiswaList.size() - 1);
                tableModel.setRowCount(0);
                for (int i = 0; i < mahasiswaList.size(); i++) {
                    Mahasiswa m = mahasiswaList.get(i);
                    tableModel.addRow(new Object[]{i + 1, m.nama, m.ipk});
                }
                JOptionPane.showMessageDialog(frame, "Data berhasil diurutkan!");
            }
        });

        frame.setVisible(true);
    }
}
# Project-Akhir
