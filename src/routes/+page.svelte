<script lang="ts">
    const notes: string[] = ["A", "A#/Bb", "B", 
                                "C", "C#/Db", "D", 
                                "D#/Eb", "E", "F", 
                                "F#/Gb", "G", "G#/Ab"]

    let noteValues: any = {
        "A": "",
        "A#/Bb": "",
        "B": "",
        "C": "",
        "C#/Db": "", 
        "D": "", 
        "D#/Eb": "", 
        "E": "", 
        "F": "", 
        "F#/Gb": "", 
        "G": "", 
        "G#/Ab": ""
    };

    const colorArray: string[] = ["red", "black", 
                                    "black", "black"]
    
    const intervals: any = {
        maj: [0, 4, 7],
        min: [0, 3, 7],
        sus2: [0, 2, 7],
        sus4: [0, 5, 7],
        dim: [0, 3, 6],
        aug: [0, 4, 8],
        maj7: [0, 4, 7, 11],
        'min/maj7': [0, 3, 7, 11],
        min7: [0, 3, 7, 10],
        dom7: [0, 4, 7, 10],
        'min7 b5': [0, 3, 6, 10],
        dim7: [0, 3, 6, 9],
        'dom7 b5': [0, 4, 6, 10],
        aug7: [0, 4, 8, 10],
        "aug maj7": [0, 4, 8, 11],
        '6': [0, 4, 7, 9],
        '9': [2],
        '11': [5],
        '13': [9],
        'Major, Ionian Mode, I (scale)': [0, 2, 4, 5, 7, 9, 11],
        'Dorian Mode, ii (scale)': [0, 2, 3, 5, 7, 9, 10],
        'Phrygian Mode, iii (scale)': [0, 1, 3, 5, 7, 8, 10],
        'Lydian Mode, IV (scale)': [0, 2, 4, 6, 7, 9, 11],
        'Mixolydian Mode, V (scale)': [0, 2, 4, 5, 7, 9, 10],
        'Natural Minor, Aeolian Mode, vi (scale)': [0, 2, 3, 5, 7, 8, 10],
        'Locrian Mode, vii (scale)': [0, 1, 3, 5, 6, 8, 10],
        'Harmonic Minor (scale)': [0, 2, 3, 5, 7, 8, 11],
        'Major Pentatonic (scale)': [0, 2, 4, 7, 9],
        'Minor Pentatonic (scale)': [0, 3, 5, 7, 10],
        'Major Blues (scale)': [0, 2, 3, 4, 7, 9],
        'Minor Blues (scale)': [0, 3, 5, 6, 7, 10]
    }

    const displayIntervals: any = {
        maj: ["1", "3", "5"],
        min: ["1", "b3", "5"],
        sus2: ["1", "2", "5"],
        sus4: ["1", "4", "5"],
        dim: ["1", "b3", "b5"],
        aug: ["1", "3", "#5"],
        maj7: ["1", "3", "5", "7"],
        'min/maj7': ["1", "b3", "5", "7"],
        min7: ["1", "b3", "5", "b7"],
        dom7: ["1", "3", "5", "b7"],
        'min7 b5': ["1", "b3", "b5", "b7"],
        dim7: ["1", "b3", "b5", "bb7"],
        'dom7 b5': ["1", "3", "b5", "b7"],
        aug7: ["1", "3", "#5", "b7"],
        'aug maj7': ["1", "3", "#5", "7"],
        '6': ["1", "3", "5", "6"],
        'Major, Ionian Mode, I (scale)': ["1", "2", "3", "4", "5", "6", "7"],
        'Dorian Mode, ii (scale)': ["1", "2", "b3", "4", "5", "6", "b7"],
        'Phrygian Mode, iii (scale)': ["1", "b2", "b3", "4", "5", "b6", "b7"],
        'Lydian Mode, IV (scale)': ["1", "2", "3", "#4", "5", "6", "7"],
        'Mixolydian Mode, V (scale)': ["1", "2", "3", "4", "5", "6", "b7"],
        'Natural Minor, Aeolian Mode, vi (scale)': ["1", "2", "b3", "4", "5", "b6", "b7"],
        'Locrian Mode, vii (scale)': ["1", "b2", "b3", "4", "b5", "b6", "b7"],
        'Harmonic Minor (scale)': ["1", "2", "b3", "4", "5", "b6", "7"],
        'Major Pentatonic (scale)': ["1", "2", "3", "5", "6"],
        'Minor Pentatonic (scale)': ["1", "b3", "4", "5", "b7"],
        'Major Blues (scale)': ["1", "2", "b3", "3", "5", "6"],
        'Minor Blues (scale)': ["1", "b3", "4", "b5", "5", "b7"]
    }

    const chord: any = {
        chordNote: '',
        chordType: '',
        chordQuality: '',
        nine: '',
        eleven: '',
        thirteen: ''
    }

    let notesInChord: string[] = []
    let currentChord: string = ''
    let notesInChordString: string = ''
    
    function getChord(){
        let root: string = chord.chordNote.trim() + chord.chordType.trim()
        let rootIndex: number = -1
        if(chord.chordQuality.trim() == 'sus2' && chord.nine == '(9)'){
            chord.nine = ''
        }
        if(chord.chordQuality.trim() == 'sus4' && chord.eleven == '(11)'){
            chord.eleven = ''
        }
        let extensions: string[] = [chord.nine.trim(), chord.eleven.trim(), chord.thirteen.trim()]
        
        for (const index in notes) {
            if (notes[index]== root){
                rootIndex = parseInt(index)
                break
            }
        }
        if(rootIndex == -1){
            for (const index in notes) {
            if (notes[index].includes(root)){
                rootIndex = parseInt(index)
                break
            }
        }
        }
        
        if (rootIndex == -1){
            alert("Invalid Root")
            chord.chordNote = chord.chordQuality = 
                            chord.chordType = 
                            chord.eleven = 
                            chord.nine = 
                            chord.thirteen = root = ''              
        }



        
        
        currentChord = chord.chordNote + 
                        chord.chordType + 
                        chord.chordQuality + 
                        chord.nine +
                        chord.eleven + 
                        chord.thirteen

        notesInChord = getNotes(rootIndex, chord.chordQuality, extensions)  
        showNotes(notesInChord, chord.chordQuality, extensions)      
    }

    function getNotes(rootIndex: number, chordQuality: string, extensions: string[] = []){
        let noteIndex = rootIndex
        chordQuality = chordQuality.trim()
        let chordIntervals: number[] = intervals[chordQuality]
        notesInChord = []
        if(extensions.length > 0){
            for (let i = 0; i < extensions.length; i++){
            switch(extensions[i]) {
                case "(9)":    
                    if(chordQuality != 'sus2'){
                        chordIntervals = [...chordIntervals, intervals["9"][0]] 
                    }                            
                    break;
                case "(#9)":            
                    chordIntervals = [...chordIntervals, intervals["9"][0] + 1] 
                    break;
                case "(b9)":            
                    chordIntervals = [...chordIntervals, intervals["9"][0] - 1] 
                    break;
                case "(11)":    
                    if(chordQuality != 'sus4'){        
                        chordIntervals = [...chordIntervals, intervals["11"][0]]
                    }
                    break;
                case "(#11)":            
                    chordIntervals = [...chordIntervals, intervals["11"][0] + 1]
                    break;
                case "(b11)":            
                    chordIntervals = [...chordIntervals, intervals["11"][0] - 1]
                    break;
                case "(13)":            
                    chordIntervals = [...chordIntervals, intervals["13"][0]]
                    break;
                case "(#13)":            
                    chordIntervals = [...chordIntervals, intervals["13"][0] + 1]
                    break;
                case "(b13)":            
                    chordIntervals = [...chordIntervals, intervals["13"][0] - 1]
                    break;           
                default:            
                    break;         
                } 
            }
        }        
        
        if(chordQuality != ''){
            for (let i = 0; i < chordIntervals.length; i++){
                notesInChord = [...notesInChord, notes[noteIndex]]
                noteIndex = rootIndex + chordIntervals[i+1]
                if (noteIndex > 11){
                noteIndex = noteIndex%12
                }
            }  
        }
        else{
            notesInChord = [...notesInChord, notes[rootIndex]]
        }
        return notesInChord;
    }  
    
    
    function showNotes(currentNotes: string[], chordQuality: string, extensions: string[] = []){
        chordQuality = chordQuality.trim()   
        let extensionsColor = "purple"
        for (let i = 0; i < currentNotes.length; i++){
            let dotNodes = document.getElementsByName(currentNotes[i])
            if (i < displayIntervals[chordQuality].length){
                noteValues[currentNotes[i]] = displayIntervals[chordQuality][i]
            }
            else{
                for (let k = 0; k < extensions.length; k++){
                    if(extensions[k] != ''){
                        noteValues[currentNotes[i]] = extensions[k]
                        extensions[k] = ''
                        break
                    }
                }   
            }            
            for (let j = 0; j < dotNodes.length; j++){
                dotNodes[j].style.visibility = ""
                if (dotNodes[j].id == "fret marker"){
                    if(i < displayIntervals[chordQuality].length){
                        dotNodes[j].style.fill = colorArray[i]
                    }
                    else{
                        dotNodes[j].style.fill = extensionsColor
                    }
                                       
                }
            }           
        }
    }    

    function clearForm(){
        location.reload()     
    }

    function clearNotes(currentNotes: string[]){
        for (let i = 0; i < currentNotes.length; i++){
            let nodes = document.getElementsByName(currentNotes[i])           
            for (let j = 0; j < nodes.length; j++){
                nodes[j].style.visibility = "hidden"
                nodes[j].style.fill = ''
            }
        }
    }

    function formSubmit(){
        clearNotes(notes)
        getChord()
        notesInChordString = ''
        for (let i = 0; i < notesInChord.length; i ++){
            notesInChordString += notesInChord[i] + ",   "
        }

    }
</script>

<body>
<h1>Chord/Scale Generator</h1>
<div>
    <h2>Chord/Scale: {currentChord}</h2>
    <h2>Notes:{" " + notesInChordString}</h2>
</div>
<hr /> 
<svg viewBox="-15 -20 650 120">
    <circle cx="45" cy="-15" r="5" fill="red"/>
    <circle cx="115" cy="-15" r="5" fill="black"/>
    <circle cx="180" cy="-15" r="5" fill="purple"/>
    <text x="26" y="-14" font-size="10" text-anchor="middle" dominant-baseline="middle">Root:</text>
	<text x="82" y="-14" font-size="10" text-anchor="middle" dominant-baseline="middle">Other Notes:</text>
	<text x="150" y="-14" font-size="10" text-anchor="middle" dominant-baseline="middle">Extensions:</text>
    <circle cx="114" cy="42.5" fill="grey" r="5" opacity="0.5"/>
    <circle cx="195" cy="42.5" fill="grey" r="5" opacity="0.5"/>
    <circle cx="271" cy="42.5" fill="grey" r="5" opacity="0.5"/>
    <circle cx="345.5" cy="42.5" fill="grey" r="5" opacity="0.5"/>
    <circle cx="549" cy="42.5" fill="grey" r="5" opacity="0.5"/>
    <circle cx="449" cy="27.5" fill="grey" r="5" opacity="0.5"/>
    <circle cx="449" cy="57" fill="grey" r="5" opacity="0.5"/>
	<g stroke="grey">
		<line x1="2" x2="2" y1="1" y2="84" stroke-width="12" />
		<line x1="50" x2="50" y1="1" y2="84" stroke-width="4" />
		<line x1="94" x2="94" y1="1" y2="84" stroke-width="4" />
		<line x1="136" x2="136" y1="1" y2="84" stroke-width="4" />
		<line x1="174" x2="174" y1="1" y2="84" stroke-width="4" />
		<line x1="216" x2="216" y1="1" y2="84" stroke-width="4" />
		<line x1="252" x2="252" y1="1" y2="84" stroke-width="4" />
		<line x1="290" x2="290" y1="1" y2="84" stroke-width="4" />
		<line x1="328" x2="328" y1="1" y2="84" stroke-width="4" />
		<line x1="362" x2="362" y1="1" y2="84" stroke-width="4" />
		<line x1="399" x2="399" y1="1" y2="84" stroke-width="4" />
		<line x1="432" x2="432" y1="1" y2="84" stroke-width="4" />
		<line x1="466" x2="466" y1="1" y2="84" stroke-width="4" />
        <line x1="500" x2="500" y1="1" y2="84" stroke-width="4" />
        <line x1="533" x2="533" y1="1" y2="84" stroke-width="4" />
        <line x1="565" x2="565" y1="1" y2="84" stroke-width="4" />
        <line x1="599" x2="599" y1="1" y2="84" stroke-width="4" />
		<line x1="0" x2="610" y1="3" y2="3" stroke-width=".75" />
		<line x1="0" x2="610" y1="20" y2="20" stroke-width="1" />
		<line x1="0" x2="610" y1="35" y2="35" stroke-width="1.5" />
		<line x1="0" x2="610" y1="50" y2="50" stroke-width="2" />
		<line x1="0" x2="610" y1="65" y2="65" stroke-width="2.5" />
		<line x1="0" x2="610" y1="80" y2="80" stroke-width="3" />
	</g>
	<text x="27.5" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">1</text>
	<text x="72" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">2</text>
	<text x="114" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">3</text>
	<text x="155" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">4</text>
	<text x="195" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">5</text>
	<text x="234" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">6</text>
	<text x="271" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">7</text>
	<text x="309" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">8</text>
	<text x="345.5" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">9</text>
	<text x="380" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">10</text>
	<text x="415" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">11</text>
	<text x="449" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">12</text>
    <text x="483" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">13</text>
	<text x="517" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">14</text>
	<text x="549" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">15</text>
	<text x="582" y="95" font-size="10" text-anchor="middle" dominant-baseline="middle">16</text>
<!-- open strings -->
    <circle id="fret marker" cx="0" cy="3" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="0" cy="20" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="0" cy="35" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="0" cy="49.2" r="6" name="D" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="0" cy="64.6" r="6" name="A" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="0" cy="80" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <text x="0" y="3.5" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>
    <text x="0" y="20.5" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>
    <text x="0" y="35.5" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>
    <text x="0" y="49.7" font-size="7" name="D" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D"]}</text>
    <text x="0" y="65.1" font-size="7" name="A" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A"]}</text>
    <text x="0" y="80.5" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>
<!-- First Fret -->
    <circle id="fret marker" cx="27.5" cy="3" r="6" name="F" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="27.5" cy="20" r="6" name="C" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="27.5" cy="35" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="27.5" cy="49.2" r="6" name="D#/Eb" style="visibility: hidden; fill: ''"/>    
    <circle id="fret marker" cx="27.5" cy="64.6" r="6" name="A#/Bb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="27.5" cy="80" r="6" name="F" style="visibility: hidden; fill: ''"/>    
    <text x="27.5" y="3.5" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>    
    <text x="27.5" y="20.5" font-size="7" name="C" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C"]}</text>
    <text x="27.5" y="35.5" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G#/Ab"]}</text>
    <text x="27.5" y="49.7" font-size="7" name="D#/Eb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D#/Eb"]}</text>
    <text x="27.5" y="65.1" font-size="7" name="A#/Bb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A#/Bb"]}</text>
    <text x="27.5" y="80.5" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>
<!-- Second Fret -->
    <circle id="fret marker" cx="72" cy="3" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="72" cy="20" r="6" name="C#/Db" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="72" cy="35" r="6" name="A" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="72" cy="49.2" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="72" cy="64.6" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="72" cy="80" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <text x="72" y="3.5" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>    
    <text x="72" y="20.5" font-size="7" name="C#/Db" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C#/Db"]}</text>
    <text x="72" y="35.5" font-size="7" name="A" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A"]}</text>
    <text x="72" y="49.7" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>
    <text x="72" y="65.1" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>
    <text x="72" y="80.5" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>
<!-- Third Fret -->
    <circle id="fret marker" cx="114" cy="3" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="114" cy="20" r="6" name="D" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="114" cy="35" r="6" name="A#/Bb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="114" cy="49.2" r="6" name="F" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="114" cy="64.6" r="6" name="C" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="114" cy="80" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <text x="114" y="3.5" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>    
    <text x="114" y="20.5" font-size="7" name="D" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D"]}</text>
    <text x="114" y="35.5" font-size="7" name="A#/Bb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A#/Bb"]}</text>
    <text x="114" y="49.7" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>
    <text x="114" y="65.1" font-size="7" name="C" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C"]}</text>
    <text x="114" y="80.5" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>
<!-- Fourth Fret -->
    <circle id="fret marker" cx="155" cy="3" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="155" cy="20" r="6" name="D#/Eb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="155" cy="35" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="155" cy="49.2" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="155" cy="64.6" r="6" name="C#/Db" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="155" cy="80" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <text x="155" y="3.5" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G#/Ab"]}</text>    
    <text x="155" y="20.5" font-size="7" name="D#/Eb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D#/Eb"]}</text>
    <text x="155" y="35.5" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>
    <text x="155" y="49.7" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>
    <text x="155" y="65.1" font-size="7" name="C#/Db" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C#/Db"]}</text>
    <text x="155" y="80.5" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G#/Ab"]}</text>
<!-- Fifth Fret -->
    <circle id="fret marker" cx="195" cy="3" r="6" name="A" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="195" cy="20" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="195" cy="35" r="6" name="C" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="195" cy="49.2" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="195" cy="64.6" r="6" name="D" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="195" cy="80" r="6" name="A" style="visibility: hidden; fill: ''"/>
    <text x="195" y="3.5" font-size="7" name="A" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A"]}</text>    
    <text x="195" y="20.5" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>
    <text x="195" y="35.5" font-size="7" name="C" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C"]}</text>
    <text x="195" y="49.7" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>
    <text x="195" y="65.1" font-size="7" name="D" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D"]}</text>
    <text x="195" y="80.5" font-size="7" name="A" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A"]}</text>
<!-- Sixth Fret -->
    <circle id="fret marker" cx="234" cy="3" r="6" name="A#/Bb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="234" cy="20" r="6" name="F" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="234" cy="35" r="6" name="C#/Db" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="234" cy="49.2" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="234" cy="64.6" r="6" name="D#/Eb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="234" cy="80" r="6" name="A#/Bb" style="visibility: hidden; fill: ''"/>
    <text x="234" y="3.5" font-size="7" name="A#/Bb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A#/Bb"]}</text>    
    <text x="234" y="20.5" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>
    <text x="234" y="35.5" font-size="7" name="C#/Db" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C#/Db"]}</text>
    <text x="234" y="49.7" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G#/Ab"]}</text>
    <text x="234" y="65.1" font-size="7" name="D#/Eb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D#/Eb"]}</text>
    <text x="234" y="80.5" font-size="7" name="A#/Bb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A#/Bb"]}</text>
<!-- Seventh Fret -->
    <circle id="fret marker" cx="271" cy="3" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="271" cy="20" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="271" cy="35" r="6" name="D" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="271" cy="49.2" r="6" name="A" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="271" cy="64.6" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="271" cy="80" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <text x="271" y="3.5" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>    
    <text x="271" y="20.5" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>
    <text x="271" y="35.5" font-size="7" name="D" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D"]}</text>
    <text x="271" y="49.7" font-size="7" name="A" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A"]}</text>
    <text x="271" y="65.1" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>
    <text x="271" y="80.5" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>
<!-- Eighth Fret -->
    <circle id="fret marker" cx="309" cy="3" r="6" name="C" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="309" cy="20" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="309" cy="35" r="6" name="D#/Eb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="309" cy="49.2" r="6" name="A#/Bb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="309" cy="64.6" r="6" name="F" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="309" cy="80" r="6" name="C" style="visibility: hidden; fill: ''"/>
    <text x="309" y="3.5" font-size="7" name="C" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C"]}</text>    
    <text x="309" y="20.5" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>
    <text x="309" y="35.5" font-size="7" name="D#/Eb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D#/Eb"]}</text>
    <text x="309" y="49.7" font-size="7" name="A#/Bb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A#/Bb"]}</text>
    <text x="309" y="65.1" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>
    <text x="309" y="80.5" font-size="7" name="C" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C"]}</text>
<!-- Nineth Fret -->
    <circle id="fret marker" cx="345.5" cy="3" r="6" name="C#/Db" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="345.5" cy="20" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="345.5" cy="35" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="345.5" cy="49.2" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="345.5" cy="64.6" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="345.5" cy="80" r="6" name="C#/Db" style="visibility: hidden; fill: ''"/>
    <text x="345.5" y="3.5" font-size="7" name="C#/Db" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C#/Db"]}</text>    
    <text x="345.5" y="20.5" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G#/Ab"]}</text>
    <text x="345.5" y="35.5" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>
    <text x="345.5" y="49.7" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>
    <text x="345.5" y="65.1" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>
    <text x="345.5" y="80.5" font-size="7" name="C#/Db" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C#/Db"]}</text>
<!-- Tenth Fret -->
    <circle id="fret marker" cx="380" cy="3" r="6" name="D" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="380" cy="20" r="6" name="A" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="380" cy="35" r="6" name="F" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="380" cy="49.2" r="6" name="C" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="380" cy="64.6" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="380" cy="80" r="6" name="D" style="visibility: hidden; fill: ''"/>
    <text x="380" y="3.5" font-size="7" name="D" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D"]}</text>    
    <text x="380" y="20.5" font-size="7" name="A" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A"]}</text>
    <text x="380" y="35.5" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>
    <text x="380" y="49.7" font-size="7" name="C" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C"]}</text>
    <text x="380" y="65.1" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>
    <text x="380" y="80.5" font-size="7" name="D" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D"]}</text>
<!-- Eleventh Fret -->
    <circle id="fret marker" cx="415" cy="3" r="6" name="D#/Eb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="415" cy="20" r="6" name="A#/Bb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="415" cy="35" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="415" cy="49.2" r="6" name="C#/Db" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="415" cy="64.6" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="415" cy="80" r="6" name="D#/Eb" style="visibility: hidden; fill: ''"/>
    <text x="415" y="3.5" font-size="7" name="D#/Eb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D#/Eb"]}</text>    
    <text x="415" y="20.5" font-size="7" name="A#/Bb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A#/Bb"]}</text>
    <text x="415" y="35.5" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>
    <text x="415" y="49.7" font-size="7" name="C#/Db" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C#/Db"]}</text>
    <text x="415" y="65.1" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G#/Ab"]}</text>
    <text x="415" y="80.5" font-size="7" name="D#/Eb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D#/Eb"]}</text>
<!-- Twelfth Fret -->
    <circle id="fret marker" cx="449" cy="3" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="449" cy="20" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="449" cy="35" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="449" cy="49.2" r="6" name="D" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="449" cy="64.6" r="6" name="A" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="449" cy="80" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <text x="449" y="3.5" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>    
    <text x="449" y="20.5" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>
    <text x="449" y="35.5" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>
    <text x="449" y="49.7" font-size="7" name="D" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D"]}</text>
    <text x="449" y="65.1" font-size="7" name="A" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A"]}</text>
    <text x="449" y="80.5" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>
<!-- Thirteenth Fret -->
    <circle id="fret marker" cx="483" cy="3" r="6" name="F" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="483" cy="20" r="6" name="C" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="483" cy="35" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="483" cy="49.2" r="6" name="D#/Eb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="483" cy="64.6" r="6" name="A#/Bb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="483" cy="80" r="6" name="F" style="visibility: hidden; fill: ''"/>
    <text x="483" y="3.5" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>    
    <text x="483" y="20.5" font-size="7" name="C" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C"]}</text>
    <text x="483" y="35.5" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G#/Ab"]}</text>
    <text x="483" y="49.7" font-size="7" name="D#/Eb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D#/Eb"]}</text>
    <text x="483" y="65.1" font-size="7" name="A#/Bb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A#/Bb"]}</text>
    <text x="483" y="80.5" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>
<!-- Fourteenth Fret -->
    <circle id="fret marker" cx="517" cy="3" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="517" cy="20" r="6" name="C#/Db" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="517" cy="35" r="6" name="A" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="517" cy="49.2" r="6" name="E" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="517" cy="64.6" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="517" cy="80" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <text x="517" y="3.5" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>    
    <text x="517" y="20.5" font-size="7" name="C#/Db" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C#/Db"]}</text>
    <text x="517" y="35.5" font-size="7" name="A" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A"]}</text>
    <text x="517" y="49.7" font-size="7" name="E" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["E"]}</text>
    <text x="517" y="65.1" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>
    <text x="517" y="80.5" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>
<!-- Fifteenth Fret -->
    <circle id="fret marker" cx="549" cy="3" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="549" cy="20" r="6" name="D" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="549" cy="35" r="6" name="A#/Bb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="549" cy="49.2" r="6" name="F" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="549" cy="64.6" r="6" name="C" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="549" cy="80" r="6" name="G" style="visibility: hidden; fill: ''"/>
    <text x="549" y="3.5" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>    
    <text x="549" y="20.5" font-size="7" name="D" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D"]}</text>
    <text x="549" y="35.5" font-size="7" name="A#/Bb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["A#/Bb"]}</text>
    <text x="549" y="49.7" font-size="7" name="F" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F"]}</text>
    <text x="549" y="65.1" font-size="7" name="C" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C"]}</text>
    <text x="549" y="80.5" font-size="7" name="G" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G"]}</text>
<!-- Sixteenth Fret -->
    <circle id="fret marker" cx="582" cy="3" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="582" cy="20" r="6" name="D#/Eb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="582" cy="35" r="6" name="B" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="582" cy="49.2" r="6" name="F#/Gb" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="582" cy="64.6" r="6" name="C#/Db" style="visibility: hidden; fill: ''"/>
    <circle id="fret marker" cx="582" cy="80" r="6" name="G#/Ab" style="visibility: hidden; fill: ''"/>
    <text x="582" y="3.5" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white' >{noteValues["G#/Ab"]}</text>    
    <text x="582" y="20.5" font-size="7" name="D#/Eb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["D#/Eb"]}</text>
    <text x="582" y="35.5" font-size="7" name="B" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["B"]}</text>
    <text x="582" y="49.7" font-size="7" name="F#/Gb" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["F#/Gb"]}</text>
    <text x="582" y="65.1" font-size="7" name="C#/Db" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["C#/Db"]}</text>
    <text x="582" y="80.5" font-size="7" name="G#/Ab" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'>{noteValues["G#/Ab"]}</text>
</svg>
<hr />
<form class="form" id="ChordForm" on:submit|preventDefault={formSubmit}>
    <label for="ChordNote">Choose a Chord Note:</label>
    <select id="ChordNote" name="ChordNote" bind:value={chord.chordNote}>
        <option value="">Choose a Note</option>
        <option value="A ">A</option>
        <option value="B ">B</option>
        <option value="C ">C</option>
        <option value="D ">D</option>
        <option value="E ">E</option>
        <option value="F ">F</option>
        <option value="G ">G</option>
    </select>
    <br />

    <label for="ChordType">Choose a Chord Type:</label>
    <select id="ChordType" name="ChordType" bind:value={chord.chordType}>
        <option value="">Natural</option>
        <option value="# ">#</option>
        <option value="b ">b</option>
    </select>
    <br />
    <label for="ChordQuality">Choose a Chord/Scale Quality:</label>
    <select id="ChordQuality" name="ChordQuality" bind:value={chord.chordQuality}>
        <option value="">Choose a Quality</option>
        <option value="maj ">Major</option>
        <option value="min ">Minor</option>
        <option value="sus2 ">Sus2</option>
        <option value="sus4 ">Sus4</option>
        <option value="dim ">Diminished</option>
        <option value="aug ">Augmented</option>
        <option value="maj7 ">Major 7th</option>
        <option value="min/maj7 ">Minor/Major 7th</option>
        <option value="min7 ">Minor 7th</option>
        <option value="dom7 ">Dominant 7th</option>
        <option value="min7 b5 ">Half-Diminished</option>
        <option value="dim7 ">Diminished 7th</option>
        <option value="dom7 b5">Dominant 7th Flat 5</option>
        <option value="aug maj7 ">Augmented Major 7th</option>
        <option value="aug7 ">Augmented 7th</option>
        <option value="6 ">6th</option>
        <option value="Major, Ionian Mode, I (scale) ">Major, Ionian Mode, I (scale)</option>
        <option value="Dorian Mode, ii (scale) ">Dorian Mode, ii (scale)</option>
        <option value="Phrygian Mode, iii (scale) ">Phrygian Mode, iii (scale)</option>
        <option value="Lydian Mode, IV (scale) ">Lydian Mode, IV (scale)</option>
        <option value="Mixolydian Mode, V (scale) ">Mixolydian Mode, V (scale)</option>
        <option value="Natural Minor, Aeolian Mode, vi (scale) ">Natural Minor, Aeolian Mode, vi (scale)</option>
        <option value="Locrian Mode, vii (scale) ">Locrian Mode, vii (scale)</option>
        <option value="Harmonic Minor (scale) ">Harmonic Minor (scale)</option>
        <option value="Major Pentatonic (scale)">Major Pentatonic (scale)</option>
        <option value="Minor Pentatonic (scale) ">Minor Pentatonic (scale)</option>
        <option value="Major Blues (scale) ">Major Blues (scale)</option>
        <option value="Minor Blues (scale) ">Minor Blues (scale)</option>
    </select>
    <br />

    <label for="ChordExtension">Choose a Chord Extension:</label>
    
    <div class="extensions">
        <div>
            <form id="Extensions">
                <label for="nineth">9th</label>
                <input type="radio" name="extensionQuality9" value="(9) " bind:group={chord.nine}/>
                <label for="natural">Natural</label>
                
                <input type="radio" name="extensionQuality9" value="(#9) " bind:group={chord.nine}/>
                <label for="sharp">#</label>

                <input type="radio" name="extensionQuality9" value="(b9) " bind:group={chord.nine}/>
                <label for="flat">b</label>
                
                <input type="radio" name="extensionQuality9" value="" bind:group={chord.nine}/>
                <label for="none">None</label><br />

                <label for="eleventh">11th</label>
                <input type="radio" name="extensionQuality11" value="(11) " bind:group={chord.eleven}/>
                <label for="natural">Natural</label>

                <input type="radio" name="extensionQuality11" value="(#11) " bind:group={chord.eleven}/>
                <label for="sharp">#</label>

                <input type="radio" name="extensionQuality11" value="(b11) " bind:group={chord.eleven}/>
                <label for="flat">b</label>
                
                <input type="radio" name="extensionQuality11" value="" bind:group={chord.eleven}/>
                <label for="none">None</label><br />

                <label for="thirteenth">13th</label>
                <input type="radio" name="extensionQuality13" value="(13) " bind:group={chord.thirteen}/>
                <label for="natural">Natural</label>

                <input type="radio" name="extensionQuality13" value="(#13) " bind:group={chord.thirteen}/>
                <label for="sharp">#</label>

                <input type="radio" name="extensionQuality13" value="(b13) " bind:group={chord.thirteen}/>
                <label for="flat">b</label>
                
                <input type="radio" name="extensionQuality13" value="" bind:group={chord.thirteen}/>
                <label for="none">None</label><br />
            </form>
        </div>
    </div>
    
    <div>
        <button type="submit">Create Chord/Scale</button>
        <button on:click={clearForm}>Clear Form</button>
    </div>
    
    

    
</form>
<hr />
<div>
    <h2><u>Helpful Reference for Extended Chords:</u></h2>    
    <h2>9ths:</h2>
    <table style="width:75%">
        <tr>
            <th>Name</th>
            <th>Quality of 9th</th>
            <th>9th Notes (on C)</th>
        </tr>
        <tr>
            <td>Major 9th</td>
            <th>9</th>
            <th>C–E–G–B–D</th>
        </tr>
        <tr>
            <td>Dominant 9th</td>
            <th>9</th>
            <th>C–E–G–B♭–D</th>
        </tr>
        <tr>
            <td>Dominant Minor 9th</td>
            <th>b9</th>
            <th>C–E–G–B♭–D♭</th>
        </tr>
        <tr>
            <td>Minor-Major 9th</td>
            <th>9</th>
            <th>C–E♭–G–B–D</th>
        </tr>
        <tr>
            <td>Minor 9th</td>
            <th>9</th>
            <th>C–E♭–G–B♭–D</th>
        </tr>
        <tr>
            <td>Augmented Major 9th</td>
            <th>9</th>
            <th>C–E–G♯–B–D</th>
        </tr>
        <tr>
            <td>Augmented Dominant 9th</td>
            <th>9</th>
            <th>C–E–G♯–B♭–D</th>
        </tr>
        <tr>
            <td>Half-Diminished 9th</td>
            <th>9</th>
            <th>C–E♭–G♭–B♭–D</th>
        </tr>
        <tr>
            <td>Half-Diminished Minor 9th</td>
            <th>b9</th>
            <th>C–E♭–G♭–B♭–D♭</th>
        </tr>
        <tr>
            <td>Diminished 9th</td>
            <th>9</th>
            <th>C–E♭–G♭–B♭♭–D</th>
        </tr>
        <tr>
            <td>Diminished Minor 9th</td>
            <th>b9</th>
            <th>C–E♭–G♭–B♭♭–D♭</th>
        </tr>
    </table>    
    <h2>11ths:</h2>
    <table style="width:75%">
        <tr>
            <th>Name</th>
            <th>Quality of 11th</th>
            <th>11th Notes (on C)</th>
        </tr>
        <tr>
            <td>11th</td>
            <th>11</th>
            <th>C–E–G–B♭–D–F</th>
        </tr>
        <tr>
            <td>Major 11th</td>
            <th>11</th>
            <th>C–E–G–B–D–F</th>
        </tr>
        <tr>
            <td>Minor Major 11th</td>
            <th>11</th>
            <th>C–E♭–G–B–D–F</th>
        </tr>
        <tr>
            <td>Minor 11th</td>
            <th>11</th>
            <th>C–E♭–G–B♭–D–F</th>
        </tr>
        <tr>
            <td>Augmented Major 11th</td>
            <th>11</th>
            <th>C–E–G♯–B–D–F</th>
        </tr>
        <tr>
            <td>Augmented 11th</td>
            <th>11</th>
            <th>C–E–G♯–B♭–D–F</th>
        </tr>
        <tr>
            <td>Half-Diminished 11th</td>
            <th>11</th>
            <th>C–E♭–G♭–B♭–D–F</th>
        </tr>
        <tr>
            <td>Diminished 11th</td>
            <th>11</th>
            <th>C–E♭–G♭–B♭♭–D–F</th>
        </tr>
    </table>
    <h2>13ths:</h2>
    <table style="width:75%">
        <tr>
            <th>Name</th>
            <th>Quality of 13th</th>
            <th>13th Notes (on C)</th>
        </tr>
        <tr>
            <td>Major 13th</td>
            <th>13</th>
            <th>C–E–G–B–D–F–A</th>
        </tr>
        <tr>
            <td>13th</td>
            <th>13</th>
            <th>C–E–G–B♭–D–F–A</th>
        </tr>
        <tr>
            <td>Minor Major 13th</td>
            <th>13</th>
            <th>C–E♭–G–B–D–F–A</th>
        </tr>
        <tr>
            <td>Minor 13th</td>
            <th>13</th>
            <th>C–E♭–G–B♭–D–F–A</th>
        </tr>
        <tr>
            <td>Augmented Major 13th</td>
            <th>13</th>
            <th>C–E–G♯–B–D–F–A</th>
        </tr>
        <tr>
            <td>Augmented 13th</td>
            <th>13</th>
            <th>C–E–G♯–B♭–D–F–A</th>
        </tr>
        <tr>
            <td>Half-Diminished 13th</td>
            <th>13</th>
            <th>C–E♭–G♭–B♭–D–F–A</th>
        </tr>
    </table>
</div>
</body>

<style>
    *, body{
        font-family: "Kanit", sans-serif;
        font-weight: bolder;
        background-color: #FDFFFC;
        color: #031926;   
    }
    h1 {
        font-size: 75px;
        font-weight: bolder;        
    }
    hr{
        margin: 15px;
    }
    table{
        border: 1px solid #9DBEBB;
        margin-left: 10%;
        margin-top: 10px;
        margin-bottom: 10px;
    }
    th, td{
        border: 1px solid #031926;
    }
    .extensions {
        white-space: nowrap;
        margin: 15px;
    }
    .extensions > div {
        width: 75px;
        display: inline-block;
    }
    form{
        margin-top: 2%;
    }
    #ChordNote, #ChordType, #ChordQuality{
        margin: 5px;
    }
    button{        
        background-color: #468189;
        border: 0px;
        color: #FDFFFC;
    }

</style>
