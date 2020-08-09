# broomhed-witer
A free open-source word processor suitable for all schools.
    

        @SuppressWarnings("static-access")
        @Override
        public void actionPerformed(ActionEvent e) {
            if(e.getSource().equals(frame.openMenuItem)){
                frame.fileChooser.showOpenDialog(frame);
                File f = frame.fileChooser.getSelectedFile();

                System.out.println("Command Executed: open");

                data.loadFile(f.getAbsoluteFile());

                if(data.showText() != null){                    
                    System.out.println(data.showText());                
                    frame.textArea.append(data.showText().toString());
                }
            }

            if(e.getSource().equals(frame.FontMenuItem)){
                System.out.println("font");
            }

            if(e.getSource().equals(frame.exitMenuItem)){
                dialogs.getConfirmDialog("exitWithoutSave");
            }

            if(e.getSource().equals(frame.saveMenuItem)){
                frame.fileChooser.showSaveDialog(null);
                File f = frame.fileChooser.getSelectedFile();
                String text = frame.textArea.getText();
                data.saveFile(f.getAbsolutePath()+".txt", text);
                System.out.println(f.getName());
                fileSaved = true;           
            }           
        }       
    }

    private File file;

    String text;
    String name;
    private File saveFile;
    int counter = 0;
    FileInputStream fis = null;
    FileOutputStream fout = null;
    StringBuilder sb = new StringBuilder(4096);

        fis = new FileInputStream(file);
        while ((counter = fis.read()) != -1) {
            System.out.print((char) counter);
            sb.append((char) counter);
            }

        }
        catch(IOException ex){
            System.out.println("file couldn't be opened, or was incorrect or you clicked cancel");
        }
        finally {
            try {
                if (fis != null)
                    fis.close();
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }
    }
    public StringBuilder showText(){

        return sb;

    }
    public void saveFile(String name, String text) {
        this.name = name;

        try{                
                fout = new FileOutputStream(name);  
                fout.write(text.getBytes());
                System.out.println("file saving worked");           
        }
        catch(IOException e){
            System.out.println("File failed to save or something went horribly wrong");
        }       
    }   

    public WordFrame(){
        setUI();
        addMenuBar();   
        textArea.setFont(yeah);
    }

    public void setUI(){
        this.setTitle("Word Processor");
        this.setIconImage(new ImageIcon(this.getClass().getResource("Bridge.jpg")).getImage());
        this.setSize(width, height);
        this.setLocation(0,0);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.add(scrollbar);

    }

    public void addMenuBar(){
        menubar = new JMenuBar();
        fileMenu = new JMenu(" File ");
        editMenu = new JMenu("Edit ");
        viewMenu = new JMenu("View ");

        newMenuItem = new JMenuItem("New");
        fileMenu.add(newMenuItem);
        fileMenu.addSeparator();
        fileMenu.setMnemonic('f');

        openMenuItem = new JMenuItem("Open");

        fileMenu.add(openMenuItem);

        saveMenuItem = new JMenuItem("Save");
        fileMenu.add(saveMenuItem);

        fileMenu.addSeparator();
        exitMenuItem = new JMenuItem("Exit");
        fileMenu.add(exitMenuItem);

        FontMenuItem = new JMenuItem("Font");
        editMenu.add(FontMenuItem);
        menubar.add(fileMenu);
        menubar.add(editMenu);
        menubar.add(viewMenu);      

        this.setJMenuBar(menubar);
    }

    public void setFontSize(int i){
        this.textHeight = i;
    }
    public void addListener(ActionListener listener){
        openMenuItem.addActionListener(listener);
        exitMenuItem.addActionListener(listener);
        saveMenuItem.addActionListener(listener);
    }


    public static void main(String args[]){
        @SuppressWarnings("unused")
        Main m = new Main();
    }
