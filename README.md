import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.File;

public class RadioButtonDemo extends JFrame {

   
    private JRadioButton[] radioButtons;
    private ButtonGroup buttonGroup;
    private JLabel imageLabel;
    
   
    private String[] petImages = {"bird.png", "cat.png", "dog.png", "rabbit.png", "pig.png"};
    private String[] petNames = {"Bird", "Cat", "Dog", "Rabbit", "Pig"};

    public RadioButtonDemo() {
        super("RadioButtonDemo");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(600, 400);
        setLocationRelativeTo(null); 
       
        JPanel mainPanel = new JPanel(new BorderLayout(10, 10));
        mainPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
        mainPanel.setBackground(Color.WHITE);

        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new BoxLayout(buttonPanel, BoxLayout.Y_AXIS));
        buttonPanel.setBorder(BorderFactory.createTitledBorder("Select a Pet"));
        buttonPanel.setOpaque(true);

        buttonGroup = new ButtonGroup();
        radioButtons = new JRadioButton[petImages.length];

        for (int i = 0; i < petImages.length; i++) {
            JRadioButton btn = new JRadioButton(petNames[i]);
            btn.setAlignmentX(Component.CENTER_ALIGNMENT);
            
           
            btn.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    String selectedPetName = ((JRadioButton)e.getSource()).getText();
                    
                   
                    JOptionPane.showMessageDialog(RadioButtonDemo.this, 
                        "You selected: " + selectedPetName);
                    
                    
                    updateImage(selectedPetName);
                }
            });

            buttonPanel.add(btn);
            buttonGroup.add(btn);
            radioButtons[i] = btn;
        }

      
        radioButtons[radioButtons.length - 1].setSelected(true);
        updateImage(radioButtons[radioButtons.length - 1].getText());

       
        JPanel imagePanel = new JPanel(new FlowLayout());
        imagePanel.setBorder(BorderFactory.createTitledBorder("Pet Display"));
        imagePanel.setPreferredSize(new Dimension(300, 250));
        
       
        ImageIcon initialIcon = new ImageIcon("pig.png"); 
       
        if(!new File("pig.png").exists()){
             initialIcon = null;
        }
        
        imageLabel = new JLabel(initialIcon, SwingConstants.CENTER);
        imageLabel.setPreferredSize(new Dimension(250, 250));
       
        imageLabel.setOpaque(true);
        imageLabel.setHorizontalAlignment(SwingConstants.CENTER);
        imageLabel.setVerticalAlignment(SwingConstants.CENTER);
        
        
        imageLabel.setText("Waiting for Selection..."); 

        imagePanel.add(imageLabel);

       
        mainPanel.add(buttonPanel, BorderLayout.WEST);
        mainPanel.add(imagePanel, BorderLayout.CENTER);

        getContentPane().add(mainPanel);
        setVisible(true);
    }

    private void updateImage(String petName) {
        
        imageLabel.setIcon(null);
        
       
        int index = 0;
        for(int i=0; i<petNames.length; i++){
            if(petNames[i].equals(petName)){
                index = i;
                break;
            }
        }

        
        String fileName = petImages[index];
        if (new File(fileName).exists()) {
            ImageIcon icon = new ImageIcon(fileName);
            
            Image img = icon.getImage();
            Image scaledImg = img.getScaledInstance(200, 200, Image.SCALE_SMOOTH);
            imageLabel.setIcon(new ImageIcon(scaledImg));
        } else {
           
            imageLabel.setText("No image found for " + petName + "\n(Please add '" + fileName + "')");
            imageLabel.setForeground(Color.RED);
        }
    }

    public static void main(String[] args) {
       
        SwingUtilities.invokeLater(() -> new RadioButtonDemo());
    }
}
