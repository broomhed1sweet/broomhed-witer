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
}   
import controlsPremade from './config';

const execute = (el, command, val) => {
  if (el && !command.includes('List')) {
    el.classList.toggle('active');
  }
  document.execCommand(command, false, val ? val: null);
}

export const init = (settings) => {
  const controls = controlsPremade[settings.controls];
  const ctrElement = settings.ctrElement;
  const outElement = settings.outElement;

  outElement.contentEditable = true;
  outElement.classList.add('output-el');

  ['click', 'touch'].forEach((evn) => {
    ctrElement.addEventListener(evn, (e) => {
      outElement.focus();
    });
  });

  outElement.addEventListener('keydown', event => {
    if (event.key === 'Tab') {
      event.preventDefault();
    } else if ((event.key === 'Enter' && document.queryCommandValue('formatBlock') === 'blockquote') || outElement.innerHTML === "" || outElement.innerHTML === "<br>") {
      setTimeout(() => {
        execute(null, 'formatBlock', '<div>');
      }, 0);
    }
  });

  controls.forEach(control => {
    const button = document.createElement('button');
    button.innerHTML = control.icon;
    button.title = control.title;
    button.setAttribute('type', 'button');
    button.classList.add('ctrl-btn');

    if (control.short) {
      document.addEventListener('keydown', (event) => {
        if (event.ctrlKey && event.key.toLowerCase() === control.short) {
            button.classList.toggle('active');
        }
      })
    }

    ['click', 'touch'].forEach((evn) => {
      if (control.state) {
        button.addEventListener(evn, () => execute(button, control.comName));
        ['keyup', 'mouseup'].forEach(cnt => {
          outElement.addEventListener(cnt, (e) => {
            if (document.queryCommandState(control.comName) && !control.comName.includes('List')) {
              button.classList.add('active');
            } else if (button.classList.contains('active')) {
              button.classList.remove('active');
            }
        });
      });
      } else if (control.formatBlock) {
        button.addEventListener(evn, () => execute(null, control.formatBlock, control.comName));
      } else {
        button.addEventListener(evn, () => {
          const val = (control.extra)();
          if (val) {
            execute(null, control.comName, val);
          }
        });
      }
    });

    ctrElement.append(button);
  });
}

export default {
  defaultControls: [
    {
      icon: '<b>B</b>', //fixed matching in tag surrounding icon text.
      title: 'Bold',
      comName: 'bold',
      state: true,
      short: 'b',
    }, {
      icon: '<i>I</i>',
      title: 'Italic',
      comName: 'italic',
      state: true,
      short: 'i',
    }, {
      icon: '<u>U</u>',
      title: 'Underline',
      comName: 'underline',
      state: true,
      short: 'u',
    }, {
      icon: '<b>H<sub>1</sub></b>',
      title: 'Heading 1',
      comName: '<h1>',
      formatBlock: 'formatBlock'
    }, {
      icon: '<b>H<sub>2</sub></b>',
      title: 'Heading 2',
      comName: '<h2>',
      formatBlock: 'formatBlock'
    }, {
      icon: 'P',
      title: 'Paragraph',
      comName: '<p>',
      formatBlock: 'formatBlock'
    }, {
      icon: '&#8220; &#8221;',
      title: 'Quote',
      comName: '<blockquote>',
      formatBlock: 'formatBlock'
    }, {
      icon: '&#35;',
      title: 'Ordered List',
      comName: 'insertOrderedList',
      state: true,
    }, {
      icon: '&#8226;',
      title: 'Unordered List',
      comName: 'insertUnorderedList',
      state: true,
    }, {
      icon: '&#128279;',
      title: 'Link',
      comName: 'createLink',
      extra: () => window.prompt('Enter the link URL'),
    }, {
      icon: '&#128247;',
      title: 'Image',
      comName: 'insertImage',
      extra: () => window.prompt('Enter the link URL'),
    },
  ]
