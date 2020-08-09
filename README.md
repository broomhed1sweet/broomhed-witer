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
    public void saveFile(String name, String text) {
    this.name = name;

    try (Writer writer = new BufferedWriter(new OutputStreamWriter(
            new FileOutputStream(name), StandardCharsets.UTF_8))) {

        writer.write(text);              
        writer.flush();
        System.out.println("file saving worked");           
    } catch(IOException e){
        // do at least a little more than just print a useless message
        e.printStackTrace();
        System.out.println("File failed to save or something went horribly wrong");
    }       
function richText(var1, var2) {
     document.getElementById(var1).addEventListener('click', function() {
      bold(var2);
     }, false);
    }
    function bold(target) {
     if (target != 0) {
      document.getElementById(target).contentDocument.execCommand('bold', false, null); 
      document.getElementById(target).contentWindow.focus();
     } else {
      document.getElementById('richTextField').contentDocument.execCommand('bold', false, null); 
      document.getElementById('richTextField').contentWindow.focus();
     }
    }

    function iFrameOn() {
    var rtfContainer, rtContainer, richTxt, richTxtId,
    rtf = document.querySelectorAll('div > form > iframe'), //Rich Text Field
    newPost = document.getElementById('richTextField').contentDocument.body,
    target = {}, rtfIndex = 0;
    //Turn iFrames On
    while (rtfIndex < rtf.length) {
        rtfID = rtf[rtfIndex].id;
        if (rtf[rtfIndex].contentDocument.designMode != 'On') {rtf[rtfIndex].contentDocument.designMode = 'On';}
        newPost.innerHTML = "<i style=\"color:#DDDDDD;\">What's up?</i>";   
        newPost.addEventListener('blur', function() {
            if (newPost.innerHTML == '') {newPost.innerHTML = "<i style=\"color:#DDDDDD;\">What's up?</i>";}
        }, false);
        document.getElementById('richTextField').contentWindow.addEventListener('focus', function() {
            if (newPost.innerHTML == "<i style=\"color:#DDDDDD;\">What's up?</i>") {newPost.innerHTML = '';}
        }, false);
        rtContainer = rtf[rtfIndex].nextElementSibling; //Next Element Sibling should be a div
        console.log('rtContainer is: '+rtContainer);
        richTxt = rtContainer.childNodes;
        console.log('richTxt is: '+richTxt);
        for (var i = 0; i < richTxt.length; i++) {
            if (richTxt[i].nodeType != 1 || (richTxt[i].nodeType == 1 && (richTxt[i].className == 'submit_button sbmtPost' || richTxt[i].className == ""))) {continue;}
            richTxtId = richTxt[i].id;
            target.rtfID = {};
            switch (richTxt[i].className) {
                case 'richText bold':
                    if (target.rtfID.bold != richTxtId) {
                        target.rtfID.bold = richTxtId;
                        console.log(target.rtfID.bold+' is associated with: '+rtfID);
                        richText(richTxtId, rtfID);
                    }
                    break;
                case 'richText underline':
                    if (target.rtfID.underline != richTxtId) {
                        target.rtfID.underline = richTxtId;
                        console.log(target.rtfID.underline+' is associated with: '+rtfID);
                    }
                    break;
                case 'richText italic':
                    if (target.rtfID.italic != richTxtId) {
                        target.rtfID.italic = richTxtId;
                        console.log(target.rtfID.italic+' is associated with: '+rtfID);
                        document.getElementById(target.rtfID.italic).addEventListener('click', function() {
                            richText(rtfID);
                        }, false);
                    }
                    break;
                default: 
                    console.log('Error with commenting system!');
            }
        }
        /*var obj = target.rtfID;
        for (var prop in obj) {
            if (obj.hasOwnProperty(prop)) { 
                console.log("prop: " + prop + " value: " + obj[prop]);
                switch(prop) {
                    case 'bold':
                        document.getElementById(obj[prop]).addEventListener('click', function() {
                            richText(obj, prop);
                        }, false);
                        break;
                    case 'underline':
                        document.getElementById(obj[prop]).addEventListener('click', function() {
                            Underline(obj[prop]);
                        }, false);
                        break;
                    case 'italic':
                        document.getElementById(obj[prop]).addEventListener('click', function() {
                            richText(obj, prop);
                        }, false);
                        break;
                    default: 
                        console.log('Error in for...in loop');
                }
            } else {console.log('error');}
        }*/
        rtfIndex++;
    }
    }

    BinarySearchTree *tree = new BinarySearchTree();
    vector<string> words;

    // Loop through arguments
    for (int i = 0; i < argc; ++i)
    {
        // Set file name if provided as argument
        if (string(argv[i]) == "-i" && argv[i+1] != nullptr)
            file_to_check = argv[i+1];
    }

    // If there was no file name as argument, prompt user
    if (file_to_check.empty())
    {
        cout << "File name: ";
        getline(cin, file_to_check);
        cout << endl;
    }

    // If file name is not empty, run spell checking methods
    if (!file_to_check.empty())
    {
        // Read words from dictionary.txt into file_data string
        file_data = read_file(word_dictionary);
        // Split the words and store into vector
        split_words(words, file_data);
        // Insert words into Binary Search Tree
        for (int i = 0; i < words.size(); ++i)
            stringstream(words[i]) >> *tree;

        // Store the data read from specified file
        file_data = read_file(file_to_check);
        // Split sentences and store each word in words vector
        split_words(words, file_data);
        cout << endl;

        // Loop through words vector and check if it exists in dictionary
        for (int i = 0; i < words.size(); ++i)
        {
            // Print out non-occurring words
            if (!tree->exists(words[i]))
            {
                spell_count++;
                cout << words[i] << endl;
            }
        }
        cout << endl;

        // Print the total number of spelling mistakes
        cout << spell_count << " spelling mistakes" << endl;
    } 
    else
    {
        // If still no file specified, print message and exit
        cout << "No file specified!" << endl;
        return 0;
    }

    // Free the memory
    delete tree;

    return 0;
}
