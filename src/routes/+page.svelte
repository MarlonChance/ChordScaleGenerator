<script lang="ts">
    import Piano from "../components/Piano.svelte"

    let notesInChord: string[] = []
    let currentChord: string = ''
    let notesInChordString: string = ''
    let guitarType: string = 'dots'
    let instrument: string = 'guitar'

    const notes: string[] = ["A", "A#/Bb", "B", 
                                "C", "C#/Db", "D", 
                                "D#/Eb", "E", "F", 
                                "F#/Gb", "G", "G#/Ab"]
    
    const tuningNotes: any = {
        "1st String": "E", 
        "2nd String": "B", 
        "3rd String": "G", 
        "4th String": "D", 
        "5th String": "A", 
        "6th String": "E"
    }

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
        '': [0],
        power: [0, 7],
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
        "": ["1"],
        power: ["1", "5"],
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

    function getChord(){
        let root: string = chord.chordNote.trim() + chord.chordType.trim()
        let rootIndex: number = -1
        if(chord.chordQuality.trim() == 'sus2' && chord.nine == '(9)'){
            chord.nine = ''
        }
        if(chord.chordQuality.trim() == 'sus4' && chord.eleven == '(11)'){
            chord.eleven = ''
        }               
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
                        chord.chordQuality
        if(chord.chordQuality.includes('scale')){
            chord.nine = ''
            chord.eleven = ''
            chord.thirteen = ''
        }

        if(((chord.chordQuality.includes('min') || chord.chordQuality.includes('dim')) && chord.nine.trim() == '(#9)') || (chord.chordQuality.includes('sus2') && chord.nine.trim() == '(9)')){
            chord.nine = ''
        }
        else{
            currentChord += chord.nine
        }
        if(((chord.chordQuality.includes('dim') || chord.chordQuality.includes('b5')) && chord.eleven.trim() == '(#11)') || (chord.chordQuality.includes('sus4') && chord.eleven.trim() == '(11)')){
            chord.eleven = ''
        }
        else{
            currentChord += chord.eleven
        }
        if((chord.chordQuality.includes('aug') && chord.thirteen.trim() == '(b13)') || 
                ((chord.chordQuality.includes('min7') || chord.chordQuality.includes('dom') || chord.chordQuality.includes('aug7')) && chord.thirteen.trim() == '(#13)') || 
                ((chord.chordQuality.includes('dim7') || chord.chordQuality.includes('6')) && chord.thirteen.trim() == '(13)')){
                    chord.thirteen = ''
        }
        else{
            currentChord += chord.thirteen
        }                             
        let extensions: string[] = [chord.nine.trim(), chord.eleven.trim(), chord.thirteen.trim()]        
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
                if(!chord.chordQuality.includes('scale')){
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
        for (let i = 0; i < notes.length; i++){
            
            noteValues[notes[i]] = ''
        }
        chordQuality = chordQuality.trim()   
        let extensionsColor = "purple"
        for (let i = 0; i < currentNotes.length; i++){
            let dotNodes = document.getElementsByName(currentNotes[i])
            if (i < displayIntervals[chordQuality].length){
                noteValues[currentNotes[i]] = displayIntervals[chordQuality][i]
            }
            else{
                for (let k = 0; k < extensions.length; k++){
                    if(extensions[k] != '' && noteValues[currentNotes[i]] == ''){
                        noteValues[currentNotes[i]] = extensions[k]
                        extensions[k] = ''
                        break
                    }
                }   
            }            
            for (let j = 0; j < dotNodes.length; j++){
                dotNodes[j].style.visibility = ""
                dotNodes[j].textContent = noteValues[currentNotes[i]]
                if (dotNodes[j].classList.contains("marker")){
                    if(i < displayIntervals[chordQuality].length){
                        dotNodes[j].style.fill = colorArray[i]
                    }
                    else if(dotNodes[j].style.fill == ''){
                        dotNodes[j].style.fill = extensionsColor
                    }                                       
                }
            }           
        }
    } 
    
    function setTuning(){
        let stringsArray: string[] = [];
        if(instrument == 'guitar'){
            stringsArray = ["one", "two", "three", "four", "five", "six"]
        }
        if(instrument == 'bass'){
            stringsArray = ["bassone", "basstwo", "bassthree", "bassfour"]
        } 
        let stringNoteIndex: number = 0;
        for(let i = 0; i < stringsArray.length; i++){
            let stringNodes = document.querySelectorAll(`[id^=${stringsArray[i]}]`)
            switch(stringsArray[i]){
                case("one"):
                    stringNoteIndex = notes.indexOf(tuningNotes["1st String"])
                    break;
                case("two"):
                    stringNoteIndex = notes.indexOf(tuningNotes["2nd String"])
                    break;
                case("three"):
                    stringNoteIndex = notes.indexOf(tuningNotes["3rd String"])
                    break;
                case("four"):
                    stringNoteIndex = notes.indexOf(tuningNotes["4th String"])
                    break;
                case("five"):
                    stringNoteIndex = notes.indexOf(tuningNotes["5th String"])
                    break;
                case("six"):
                    stringNoteIndex = notes.indexOf(tuningNotes["6th String"])
                    break;
                case("bassone"):
                    stringNoteIndex = notes.indexOf(tuningNotes["3rd String"])
                    break;
                case("basstwo"):
                    stringNoteIndex = notes.indexOf(tuningNotes["4th String"])
                    break;
                case("bassthree"):
                    stringNoteIndex = notes.indexOf(tuningNotes["5th String"])
                    break;
                case("bassfour"):
                    stringNoteIndex = notes.indexOf(tuningNotes["6th String"])
                    break;
            }
            for(let j = 0; j < stringNodes.length; j += 2){
                stringNodes[j].setAttribute("name", notes[stringNoteIndex])
                stringNodes[j+1].setAttribute("name", notes[stringNoteIndex])
                stringNoteIndex++
                if(stringNoteIndex == 12){
                    stringNoteIndex = 0
                }
            } 
        }
    }

    function setGuitarType(){
        let dotsMarkers = document.getElementsByName('dots')
        let trapezoidMarkers = document.getElementsByName('trapezoid')
        let sharkFinMarkers = document.getElementsByName('sharkFin')
        let sideDotsMarkers = document.getElementsByName('sideDots')
        if(guitarType == 'dots'){
            for ( let i = 0; i < dotsMarkers.length; i++){
                dotsMarkers[i].style.visibility = ''
            }
            for ( let i = 0; i < trapezoidMarkers.length; i++){
                trapezoidMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < sharkFinMarkers.length; i++){
                sharkFinMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < sideDotsMarkers.length; i++){
                sideDotsMarkers[i].style.visibility = 'hidden'
            }
        }

        if(guitarType == 'trapezoid'){
            for ( let i = 0; i < dotsMarkers.length; i++){
                dotsMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < trapezoidMarkers.length; i++){
                trapezoidMarkers[i].style.visibility = ''
            }
            for ( let i = 0; i < sharkFinMarkers.length; i++){
                sharkFinMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < sideDotsMarkers.length; i++){
                sideDotsMarkers[i].style.visibility = 'hidden'
            }
        }

        if(guitarType == 'sharkFin'){
            for ( let i = 0; i < dotsMarkers.length; i++){
                dotsMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < trapezoidMarkers.length; i++){
                trapezoidMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < sharkFinMarkers.length; i++){
                sharkFinMarkers[i].style.visibility = ''
            }
            for ( let i = 0; i < sideDotsMarkers.length; i++){
                sideDotsMarkers[i].style.visibility = 'hidden'
            }
        }
        if(guitarType == 'sideDots'){
            for ( let i = 0; i < dotsMarkers.length; i++){
                dotsMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < trapezoidMarkers.length; i++){
                trapezoidMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < sharkFinMarkers.length; i++){
                sharkFinMarkers[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < sideDotsMarkers.length; i++){
                sideDotsMarkers[i].style.visibility = ''
            }
        }
    }

    function setInstrumentType(){
        let guitar = document.getElementsByName('guitar')
        let bass = document.getElementsByName('bass')
        
        if(instrument == 'guitar'){
            for ( let i = 0; i < guitar.length; i++){
                guitar[i].style.visibility = ''
            }
            for ( let i = 0; i < bass.length; i++){
                bass[i].style.visibility = 'hidden'
            }
        }

        if(instrument == 'bass'){
            for ( let i = 0; i < guitar.length; i++){
                guitar[i].style.visibility = 'hidden'
            }
            for ( let i = 0; i < bass.length; i++){
                bass[i].style.visibility = ''
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
        setTuning()
        setInstrumentType()
        setGuitarType()
        clearNotes(notes)
        getChord()
        notesInChordString = ''
        for (let i = 0; i < notesInChord.length; i ++){
            notesInChordString += notesInChord[i] + ",   "
        }
    }
</script>

<body>
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
    <g class="trapezoid">
        <polygon name="trapezoid" points="105,71 105,12 125,22 125,61" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="trapezoid" points="184,71 184,12 204,22 204,61" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="trapezoid" points="262,71 262,12 280,22 280,61" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="trapezoid" points="336.5,71 336.5,12 352.5,22 352.5,61" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="trapezoid" points="440,71 440,12 455,22 455,61" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="trapezoid" points="540,71 540,12 555,22 555,61" fill="grey" opacity="0.5" style="visibility: hidden"/>
    </g>

    <g class="sharkFin">
        <polygon name="sharkFin" points="98,81 130,3 130,81" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="sharkFin" points="177,81 209,3 209,81" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="sharkFin" points="255.5,81 285,3 285,81" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="sharkFin" points="331.5,81 357.5,3 357.5,81" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="sharkFin" points="435,81 460,3 460,81" fill="grey" opacity="0.5" style="visibility: hidden"/>
        <polygon name="sharkFin" points="536,81 560,3 560,81" fill="grey" opacity="0.5" style="visibility: hidden"/>
    </g>

    <g class="dots">
        <circle name="dots" cx="114" cy="42.5" fill="grey" r="5" opacity="0.5" style="visibility: ''"/>
        <circle name="dots" cx="195" cy="42.5" fill="grey" r="5" opacity="0.5" style="visibility: ''"/>
        <circle name="dots" cx="271" cy="42.5" fill="grey" r="5" opacity="0.5" style="visibility: ''"/>
        <circle name="dots" cx="345.5" cy="42.5" fill="grey" r="5" opacity="0.5" style="visibility: ''"/>
        <circle name="dots" cx="549" cy="42.5" fill="grey" r="5" opacity="0.5" style="visibility: ''"/>
        <circle name="dots" cx="449" cy="27.5" fill="grey" r="5" opacity="0.5" style="visibility: ''"/>
        <circle name="dots" cx="449" cy="57" fill="grey" r="5" opacity="0.5" style="visibility: ''"/>
    </g>

    <g class="sideDots">
        <circle name="sideDots" cx="114" cy="73" fill="grey" r="3" opacity="0.5" style="visibility: hidden"/>
        <circle name="sideDots" cx="195" cy="73" fill="grey" r="3" opacity="0.5" style="visibility: hidden"/>
        <circle name="sideDots" cx="271" cy="73" fill="grey" r="3" opacity="0.5" style="visibility: hidden"/>
        <circle name="sideDots" cx="345.5" cy="73" fill="grey" r="3" opacity="0.5" style="visibility: hidden"/>
        <circle name="sideDots" cx="549" cy="73" fill="grey" r="3" opacity="0.5" style="visibility: hidden"/>
        <circle name="sideDots" cx="449" cy="57" fill="grey" r="3" opacity="0.5" style="visibility: hidden"/>
        <circle name="sideDots" cx="449" cy="73" fill="grey" r="3" opacity="0.5" style="visibility: hidden"/>
    </g>

	<g  name="guitar" style="visibility: ''" >
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
        <g class="fretNumbers">
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
        </g>
        <g class="open">
            <circle class="position marker" cx="0" cy="3" r="6" name=""  id="one0" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="0" cy="20" r="6" name=""  id="two0" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="0" cy="35" r="6" name=""  id="three0" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="0" cy="49.2" r="6" name=""  id="four0" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="0" cy="64.6" r="6" name=""  id="five0" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="0" cy="80" r="6" name=""  id="six0" style="visibility: hidden; fill: ''"/>
            <text x="0" y="3.5" font-size="7" name=""  id="one0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="0" y="20.5" font-size="7" name=""  id="two0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="0" y="35.5" font-size="7" name=""  id="three0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="0" y="49.7" font-size="7" name=""  id="four0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="0" y="65.1" font-size="7" name=""  id="five0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="0" y="80.5" font-size="7" name=""  id="six0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="one">
            <circle class="position marker" cx="27.5" cy="3" r="6" name=""  id="one1" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="27.5" cy="20" r="6" name=""  id="two1" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="27.5" cy="35" r="6" name=""  id="three1" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="27.5" cy="49.2" r="6" name=""  id="four1" style="visibility: hidden; fill: ''"/>    
            <circle class="position marker" cx="27.5" cy="64.6" r="6" name=""  id="five1" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="27.5" cy="80" r="6" name=""  id="six1" style="visibility: hidden; fill: ''"/>    
            <text x="27.5" y="3.5" font-size="7" name=""  id="one1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="27.5" y="20.5" font-size="7" name=""  id="two1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="27.5" y="35.5" font-size="7" name=""  id="three1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="27.5" y="49.7" font-size="7" name=""  id="four1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="27.5" y="65.1" font-size="7" name=""  id="five1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="27.5" y="80.5" font-size="7" name=""  id="six1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="two">
            <circle class="position marker" cx="72" cy="3" r="6" name=""  id="one2" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="72" cy="20" r="6" name=""  id="two2" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="72" cy="35" r="6" name=""  id="three2" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="72" cy="49.2" r="6" name=""  id="four2" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="72" cy="64.6" r="6" name=""  id="five2" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="72" cy="80" r="6" name=""  id="six2" style="visibility: hidden; fill: ''"/>
            <text x="72" y="3.5" font-size="7" name=""  id="one2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="72" y="20.5" font-size="7" name=""  id="two2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="72" y="35.5" font-size="7" name=""  id="three2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="72" y="49.7" font-size="7" name=""  id="four2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="72" y="65.1" font-size="7" name=""  id="five2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="72" y="80.5" font-size="7" name=""  id="six2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="three">
            <circle class="position marker" cx="114" cy="3" r="6" name=""  id="one3" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="114" cy="20" r="6" name=""  id="two3" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="114" cy="35" r="6" name=""  id="three3" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="114" cy="49.2" r="6" name=""  id="four3" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="114" cy="64.6" r="6" name=""  id="five3" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="114" cy="80" r="6" name=""  id="six3" style="visibility: hidden; fill: ''"/>
            <text x="114" y="3.5" font-size="7" name=""  id="one3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="114" y="20.5" font-size="7" name=""  id="two3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="114" y="35.5" font-size="7" name=""  id="three3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="114" y="49.7" font-size="7" name=""  id="four3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="114" y="65.1" font-size="7" name=""  id="five3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="114" y="80.5" font-size="7" name=""  id="six3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="four">
            <circle class="position marker" cx="155" cy="3" r="6" name=""  id="one4" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="155" cy="20" r="6" name=""  id="two4" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="155" cy="35" r="6" name=""  id="three4" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="155" cy="49.2" r="6" name=""  id="four4" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="155" cy="64.6" r="6" name=""  id="five4" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="155" cy="80" r="6" name=""  id="six4" style="visibility: hidden; fill: ''"/>
            <text x="155" y="3.5" font-size="7" name=""  id="one4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="155" y="20.5" font-size="7" name=""  id="two4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="155" y="35.5" font-size="7" name=""  id="three4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="155" y="49.7" font-size="7" name=""  id="four4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="155" y="65.1" font-size="7" name=""  id="five4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="155" y="80.5" font-size="7" name=""  id="six4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="five">
            <circle class="position marker" cx="195" cy="3" r="6" name=""  id="one5" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="195" cy="20" r="6" name=""  id="two5" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="195" cy="35" r="6" name=""  id="three5" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="195" cy="49.2" r="6" name=""  id="four5" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="195" cy="64.6" r="6" name=""  id="five5" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="195" cy="80" r="6" name=""  id="six5" style="visibility: hidden; fill: ''"/>
            <text x="195" y="3.5" font-size="7" name=""  id="one5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="195" y="20.5" font-size="7" name=""  id="two5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="195" y="35.5" font-size="7" name=""  id="three5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="195" y="49.7" font-size="7" name=""  id="four5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="195" y="65.1" font-size="7" name=""  id="five5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="195" y="80.5" font-size="7" name=""  id="six5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="six">
            <circle class="position marker" cx="234" cy="3" r="6" name=""  id="one6" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="234" cy="20" r="6" name=""  id="two6" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="234" cy="35" r="6" name=""  id="three6" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="234" cy="49.2" r="6" name=""  id="four6" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="234" cy="64.6" r="6" name=""  id="five6" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="234" cy="80" r="6" name=""  id="six6" style="visibility: hidden; fill: ''"/>
            <text x="234" y="3.5" font-size="7" name=""  id="one6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="234" y="20.5" font-size="7" name=""  id="two6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="234" y="35.5" font-size="7" name=""  id="three6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="234" y="49.7" font-size="7" name=""  id="four6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="234" y="65.1" font-size="7" name=""  id="five6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="234" y="80.5" font-size="7" name=""  id="six6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="seven">
            <circle class="position marker" cx="271" cy="3" r="6" name=""  id="one7" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="271" cy="20" r="6" name=""  id="two7" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="271" cy="35" r="6" name=""  id="three7" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="271" cy="49.2" r="6" name=""  id="four7" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="271" cy="64.6" r="6" name=""  id="five7" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="271" cy="80" r="6" name=""  id="six7" style="visibility: hidden; fill: ''"/>
            <text x="271" y="3.5" font-size="7" name=""  id="one7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="271" y="20.5" font-size="7" name=""  id="two7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="271" y="35.5" font-size="7" name=""  id="three7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="271" y="49.7" font-size="7" name=""  id="four7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="271" y="65.1" font-size="7" name=""  id="five7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="271" y="80.5" font-size="7" name=""  id="six7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="eight">
            <circle class="position marker" cx="309" cy="3" r="6" name=""  id="one8" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="309" cy="20" r="6" name=""  id="two8" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="309" cy="35" r="6" name=""  id="three8" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="309" cy="49.2" r="6" name=""  id="four8" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="309" cy="64.6" r="6" name=""  id="five8" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="309" cy="80" r="6" name=""  id="six8" style="visibility: hidden; fill: ''"/>
            <text x="309" y="3.5" font-size="7" name=""  id="one8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="309" y="20.5" font-size="7" name=""  id="two8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="309" y="35.5" font-size="7" name=""  id="three8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="309" y="49.7" font-size="7" name=""  id="four8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="309" y="65.1" font-size="7" name=""  id="five8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="309" y="80.5" font-size="7" name=""  id="six8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="nine">
            <circle class="position marker" cx="345.5" cy="3" r="6" name=""  id="one9" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="345.5" cy="20" r="6" name=""  id="two9" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="345.5" cy="35" r="6" name=""  id="three9" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="345.5" cy="49.2" r="6" name=""  id="four9" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="345.5" cy="64.6" r="6" name=""  id="five9" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="345.5" cy="80" r="6" name=""  id="six9" style="visibility: hidden; fill: ''"/>
            <text x="345.5" y="3.5" font-size="7" name=""  id="one9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="345.5" y="20.5" font-size="7" name=""  id="two9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="345.5" y="35.5" font-size="7" name=""  id="three9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="345.5" y="49.7" font-size="7" name=""  id="four9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="345.5" y="65.1" font-size="7" name=""  id="five9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="345.5" y="80.5" font-size="7" name=""  id="six9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="ten">
            <circle class="position marker" cx="380" cy="3" r="6" name=""  id="one10" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="380" cy="20" r="6" name=""  id="two10" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="380" cy="35" r="6" name=""  id="three10" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="380" cy="49.2" r="6" name=""  id="four10" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="380" cy="64.6" r="6" name=""  id="five10" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="380" cy="80" r="6" name=""  id="six10" style="visibility: hidden; fill: ''"/>
            <text x="380" y="3.5" font-size="7" name=""  id="one10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="380" y="20.5" font-size="7" name=""  id="two10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="380" y="35.5" font-size="7" name=""  id="three10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="380" y="49.7" font-size="7" name=""  id="four10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="380" y="65.1" font-size="7" name=""  id="five10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="380" y="80.5" font-size="7" name=""  id="six10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="eleven">
            <circle class="position marker" cx="415" cy="3" r="6" name=""  id="one11" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="415" cy="20" r="6" name=""  id="two11" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="415" cy="35" r="6" name=""  id="three11" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="415" cy="49.2" r="6" name=""  id="four11" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="415" cy="64.6" r="6" name=""  id="five11" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="415" cy="80" r="6" name=""  id="six11" style="visibility: hidden; fill: ''"/>
            <text x="415" y="3.5" font-size="7" name=""  id="one11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="415" y="20.5" font-size="7" name=""  id="two11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="415" y="35.5" font-size="7" name=""  id="three11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="415" y="49.7" font-size="7" name=""  id="four11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="415" y="65.1" font-size="7" name=""  id="five11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="415" y="80.5" font-size="7" name=""  id="six11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="twelve">
            <circle class="position marker" cx="449" cy="3" r="6" name=""  id="one12" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="449" cy="20" r="6" name=""  id="two12" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="449" cy="35" r="6" name=""  id="three12" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="449" cy="49.2" r="6" name=""  id="four12" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="449" cy="64.6" r="6" name=""  id="five12" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="449" cy="80" r="6" name=""  id="six12" style="visibility: hidden; fill: ''"/>
            <text x="449" y="3.5" font-size="7" name=""  id="one12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="449" y="20.5" font-size="7" name=""  id="two12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="449" y="35.5" font-size="7" name=""  id="three12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="449" y="49.7" font-size="7" name=""  id="four12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="449" y="65.1" font-size="7" name=""  id="five12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="449" y="80.5" font-size="7" name=""  id="six12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="thirteen">
            <circle class="position marker" cx="483" cy="3" r="6" name=""  id="one13" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="483" cy="20" r="6" name=""  id="two13" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="483" cy="35" r="6" name=""  id="three13" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="483" cy="49.2" r="6" name=""  id="four13" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="483" cy="64.6" r="6" name=""  id="five13" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="483" cy="80" r="6" name=""  id="six13" style="visibility: hidden; fill: ''"/>
            <text x="483" y="3.5" font-size="7" name=""  id="one13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="483" y="20.5" font-size="7" name=""  id="two13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="483" y="35.5" font-size="7" name=""  id="three13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="483" y="49.7" font-size="7" name=""  id="four13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="483" y="65.1" font-size="7" name=""  id="five13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="483" y="80.5" font-size="7" name=""  id="six13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="fourteen">
            <circle class="position marker" cx="517" cy="3" r="6" name=""  id="one14" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="517" cy="20" r="6" name=""  id="two14" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="517" cy="35" r="6" name=""  id="three14" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="517" cy="49.2" r="6" name=""  id="four14" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="517" cy="64.6" r="6" name=""  id="five14" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="517" cy="80" r="6" name=""  id="six14" style="visibility: hidden; fill: ''"/>
            <text x="517" y="3.5" font-size="7" name=""  id="one14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="517" y="20.5" font-size="7" name=""  id="two14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="517" y="35.5" font-size="7" name=""  id="three14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="517" y="49.7" font-size="7" name=""  id="four14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="517" y="65.1" font-size="7" name=""  id="five14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="517" y="80.5" font-size="7" name=""  id="six14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="fifteen">
            <circle class="position marker" cx="549" cy="3" r="6" name=""  id="one15" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="549" cy="20" r="6" name=""  id="two15" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="549" cy="35" r="6" name=""  id="three15" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="549" cy="49.2" r="6" name=""  id="four15" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="549" cy="64.6" r="6" name=""  id="five15" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="549" cy="80" r="6" name=""  id="six15" style="visibility: hidden; fill: ''"/>
            <text x="549" y="3.5" font-size="7" name=""  id="one15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>    
            <text x="549" y="20.5" font-size="7" name=""  id="two15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="549" y="35.5" font-size="7" name=""  id="three15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="549" y="49.7" font-size="7" name=""  id="four15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="549" y="65.1" font-size="7" name=""  id="five15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="549" y="80.5" font-size="7" name=""  id="six15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="sixteen">
            <circle class="position marker" cx="582" cy="3" r="6" name=""  id="one16" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="582" cy="20" r="6" name=""  id="two16" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="582" cy="35" r="6" name=""  id="three16" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="582" cy="49.2" r="6" name=""  id="four16" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="582" cy="64.6" r="6" name=""  id="five16" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="582" cy="80" r="6" name=""  id="six16" style="visibility: hidden; fill: ''"/>
            <text x="582" y="3.5" font-size="7" name=""  id="one16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white' ></text>    
            <text x="582" y="20.5" font-size="7" name=""  id="two16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="582" y="35.5" font-size="7" name=""  id="three16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="582" y="49.7" font-size="7" name=""  id="four16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="582" y="65.1" font-size="7" name=""  id="five16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="582" y="80.5" font-size="7" name=""  id="six16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
    </g>
    <g  name="bass" style="visibility: hidden" >
        <g stroke="grey">
            <line x1="2" x2="2" y1="1" y2="86" stroke-width="12" />
            <line x1="50" x2="50" y1="1" y2="86" stroke-width="4" />
            <line x1="94" x2="94" y1="1" y2="86" stroke-width="4" />
            <line x1="136" x2="136" y1="1" y2="86" stroke-width="4" />
            <line x1="174" x2="174" y1="1" y2="86" stroke-width="4" />
            <line x1="216" x2="216" y1="1" y2="86" stroke-width="4" />
            <line x1="252" x2="252" y1="1" y2="86" stroke-width="4" />
            <line x1="290" x2="290" y1="1" y2="86" stroke-width="4" />
            <line x1="328" x2="328" y1="1" y2="86" stroke-width="4" />
            <line x1="362" x2="362" y1="1" y2="86" stroke-width="4" />
            <line x1="399" x2="399" y1="1" y2="86" stroke-width="4" />
            <line x1="432" x2="432" y1="1" y2="86" stroke-width="4" />
            <line x1="466" x2="466" y1="1" y2="86" stroke-width="4" />
            <line x1="500" x2="500" y1="1" y2="86" stroke-width="4" />
            <line x1="533" x2="533" y1="1" y2="86" stroke-width="4" />
            <line x1="565" x2="565" y1="1" y2="86" stroke-width="4" />
            <line x1="599" x2="599" y1="1" y2="86" stroke-width="4" />
            <line x1="0" x2="610" y1="3" y2="3" stroke-width="1.5" />
            <line x1="0" x2="610" y1="28" y2="29" stroke-width="3" />
            <line x1="0" x2="610" y1="57" y2="56" stroke-width="5.5" />
            <line x1="0" x2="610" y1="80" y2="80" stroke-width="7" />
        </g>
        <g class="fretNumbers">
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
        </g>
        <g class="open">
            <circle class="positio>n marker" cx="0" cy="3" r="6" name=""  id="bassone0" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="0" cy="29" r="6" name=""  id="basstwo0" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="0" cy="56" r="6" name=""  id="bassthree0" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="0" cy="80" r="6" name=""  id="bassfour0" style="visibility: hidden; fill: ''"/>
            <text x="0" y="3.5" font-size="7" name=""  id="bassone0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="0" y="29.5" font-size="7" name=""  id="basstwo0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="0" y="56.5" font-size="7" name=""  id="bassthree0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="0" y="80.5" font-size="7" name=""  id="bassfour0" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="one">
            <circle class="position marker" cx="27.5" cy="3" r="6" name=""  id="bassone1" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="27.5" cy="29"r="6" name=""  id="basstwo1" style="visibility: hidden; fill: ''"/>    
            <circle class="position marker" cx="27.5" cy="56" r="6" name=""  id="bassthree1" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="27.5" cy="80" r="6" name=""  id="bassfour1" style="visibility: hidden; fill: ''"/>    
            <text x="27.5" y="3.5" font-size="7" name=""  id="bassone1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="27.5" y="29.5" font-size="7" name=""  id="basstwo1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="27.5" y="56.5" font-size="7" name=""  id="bassthree1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="27.5" y="80.5" font-size="7" name=""  id="bassfour1" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="two">
            <circle class="position marker" cx="72" cy="3" r="6" name=""  id="bassone2" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="72" cy="29"r="6" name=""  id="basstwo2" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="72" cy="56" r="6" name=""  id="bassthree2" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="72" cy="80" r="6" name=""  id="bassfour2" style="visibility: hidden; fill: ''"/>
            <text x="72" y="3.5" font-size="7" name=""  id="bassone2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="72" y="29.5" font-size="7" name=""  id="basstwo2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="72" y="56.5" font-size="7" name=""  id="bassthree2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="72" y="80.5" font-size="7" name=""  id="bassfour2" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="three">
            <circle class="position marker" cx="114" cy="3" r="6" name=""  id="bassone3" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="114" cy="29"r="6" name=""  id="basstwo3" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="114" cy="56" r="6" name=""  id="bassthree3" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="114" cy="80" r="6" name=""  id="bassfour3" style="visibility: hidden; fill: ''"/>
            <text x="114" y="3.5" font-size="7" name=""  id="bassone3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="114" y="29.5" font-size="7" name=""  id="basstwo3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="114" y="56.5" font-size="7" name=""  id="bassthree3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="114" y="80.5" font-size="7" name=""  id="bassfour3" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="four">
            <circle class="position marker" cx="155" cy="3" r="6" name=""  id="bassone4" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="155" cy="29"r="6" name=""  id="basstwo4" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="155" cy="56" r="6" name=""  id="bassthree4" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="155" cy="80" r="6" name=""  id="bassfour4" style="visibility: hidden; fill: ''"/>
            <text x="155" y="3.5" font-size="7" name=""  id="bassone4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="155" y="29.5" font-size="7" name=""  id="basstwo4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="155" y="56.5" font-size="7" name=""  id="bassthree4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="155" y="80.5" font-size="7" name=""  id="bassfour4" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="five">
            <circle class="position marker" cx="195" cy="3" r="6" name=""  id="bassone5" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="195" cy="29"r="6" name=""  id="basstwo5" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="195" cy="56" r="6" name=""  id="bassthree5" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="195" cy="80" r="6" name=""  id="bassfour5" style="visibility: hidden; fill: ''"/>
            <text x="195" y="3.5" font-size="7" name=""  id="bassone5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="195" y="29.5" font-size="7" name=""  id="basstwo5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="195" y="56.5" font-size="7" name=""  id="bassthree5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="195" y="80.5" font-size="7" name=""  id="bassfour5" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="six">
            <circle class="position marker" cx="234" cy="3" r="6" name=""  id="bassone6" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="234" cy="29"r="6" name=""  id="basstwo6" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="234" cy="56" r="6" name=""  id="bassthree6" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="234" cy="80" r="6" name=""  id="bassfour6" style="visibility: hidden; fill: ''"/>
            <text x="234" y="3.5" font-size="7" name=""  id="bassone6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="234" y="29.5" font-size="7" name=""  id="basstwo6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="234" y="56.5" font-size="7" name=""  id="bassthree6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="234" y="80.5" font-size="7" name=""  id="bassfour6" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="seven">
            <circle class="position marker" cx="271" cy="3" r="6" name=""  id="bassone7" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="271" cy="29"r="6" name=""  id="basstwo7" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="271" cy="56" r="6" name=""  id="bassthree7" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="271" cy="80" r="6" name=""  id="bassfour7" style="visibility: hidden; fill: ''"/>
            <text x="271" y="3.5" font-size="7" name=""  id="bassone7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="271" y="29.5" font-size="7" name=""  id="basstwo7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="271" y="56.5" font-size="7" name=""  id="bassthree7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="271" y="80.5" font-size="7" name=""  id="bassfour7" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="eight">
            <circle class="position marker" cx="309" cy="3" r="6" name=""  id="bassone8" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="309" cy="29"r="6" name=""  id="basstwo8" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="309" cy="56" r="6" name=""  id="bassthree8" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="309" cy="80" r="6" name=""  id="bassfour8" style="visibility: hidden; fill: ''"/>
            <text x="309" y="3.5" font-size="7" name=""  id="bassone8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="309" y="29.5" font-size="7" name=""  id="basstwo8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="309" y="56.5" font-size="7" name=""  id="bassthree8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="309" y="80.5" font-size="7" name=""  id="bassfour8" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="nine">
            <circle class="position marker" cx="345.5" cy="3" r="6" name=""  id="bassone9" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="345.5" cy="29"r="6" name=""  id="basstwo9" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="345.5" cy="56" r="6" name=""  id="bassthree9" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="345.5" cy="80" r="6" name=""  id="bassfour9" style="visibility: hidden; fill: ''"/>
            <text x="345.5" y="3.5" font-size="7" name=""  id="bassone9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="345.5" y="29.5" font-size="7" name=""  id="basstwo9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="345.5" y="56.5" font-size="7" name=""  id="bassthree9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="345.5" y="80.5" font-size="7" name=""  id="bassfour9" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="ten">
            <circle class="position marker" cx="380" cy="3" r="6" name=""  id="bassone10" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="380" cy="29"r="6" name=""  id="basstwo10" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="380" cy="56" r="6" name=""  id="bassthree10" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="380" cy="80" r="6" name=""  id="bassfour10" style="visibility: hidden; fill: ''"/>
            <text x="380" y="3.5" font-size="7" name=""  id="bassone10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="380" y="29.5" font-size="7" name=""  id="basstwo10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="380" y="56.5" font-size="7" name=""  id="bassthree10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="380" y="80.5" font-size="7" name=""  id="bassfour10" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="eleven">
            <circle class="position marker" cx="415" cy="3" r="6" name=""  id="bassone11" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="415" cy="29"r="6" name=""  id="basstwo11" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="415" cy="56" r="6" name=""  id="bassthree11" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="415" cy="80" r="6" name=""  id="bassfour11" style="visibility: hidden; fill: ''"/>
            <text x="415" y="3.5" font-size="7" name=""  id="bassone11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="415" y="29.5" font-size="7" name=""  id="basstwo11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="415" y="56.5" font-size="7" name=""  id="bassthree11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="415" y="80.5" font-size="7" name=""  id="bassfour11" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="twelve">
            <circle class="position marker" cx="449" cy="3" r="6" name=""  id="bassone12" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="449" cy="29"r="6" name=""  id="basstwo12" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="449" cy="56" r="6" name=""  id="bassthree12" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="449" cy="80" r="6" name=""  id="bassfour12" style="visibility: hidden; fill: ''"/>
            <text x="449" y="3.5" font-size="7" name=""  id="bassone12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="449" y="29.5" font-size="7" name=""  id="basstwo12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="449" y="56.5" font-size="7" name=""  id="bassthree12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="449" y="80.5" font-size="7" name=""  id="bassfour12" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="thirteen">
            <circle class="position marker" cx="483" cy="3" r="6" name=""  id="bassone13" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="483" cy="29"r="6" name=""  id="basstwo13" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="483" cy="56" r="6" name=""  id="bassthree13" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="483" cy="80" r="6" name=""  id="bassfour13" style="visibility: hidden; fill: ''"/>
            <text x="483" y="3.5" font-size="7" name=""  id="bassone13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="483" y="29.5" font-size="7" name=""  id="basstwo13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="483" y="56.5" font-size="7" name=""  id="bassthree13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="483" y="80.5" font-size="7" name=""  id="bassfour13" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="fourteen">
            <circle class="position marker" cx="517" cy="3" r="6" name=""  id="bassone14" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="517" cy="29"r="6" name=""  id="basstwo14" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="517" cy="56" r="6" name=""  id="bassthree14" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="517" cy="80" r="6" name=""  id="bassfour14" style="visibility: hidden; fill: ''"/>
            <text x="517" y="3.5" font-size="7" name=""  id="bassone14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="517" y="29.5" font-size="7" name=""  id="basstwo14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="517" y="56.5" font-size="7" name=""  id="bassthree14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="517" y="80.5" font-size="7" name=""  id="bassfour14" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="fifteen">
            <circle class="position marker" cx="549" cy="3" r="6" name=""  id="bassone15" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="549" cy="29"r="6" name=""  id="basstwo15" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="549" cy="56" r="6" name=""  id="bassthree15" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="549" cy="80" r="6" name=""  id="bassfour15" style="visibility: hidden; fill: ''"/>
            <text x="549" y="3.5" font-size="7" name=""  id="bassone15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="549" y="29.5" font-size="7" name=""  id="basstwo15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="549" y="56.5" font-size="7" name=""  id="bassthree15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="549" y="80.5" font-size="7" name=""  id="bassfour15" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
        <g class="sixteen">
            <circle class="position marker" cx="582" cy="3" r="6" name=""  id="bassone16" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="582" cy="29"r="6" name=""  id="basstwo16" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="582" cy="56" r="6" name=""  id="bassthree16" style="visibility: hidden; fill: ''"/>
            <circle class="position marker" cx="582" cy="80" r="6" name=""  id="bassfour16" style="visibility: hidden; fill: ''"/>
            <text x="582" y="3.5" font-size="7" name=""  id="bassone16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="582" y="29.5" font-size="7" name=""  id="basstwo16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="582" y="56.5" font-size="7" name=""  id="bassthree16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
            <text x="582" y="80.5" font-size="7" name=""  id="bassfour16" text-anchor="middle" dominant-baseline="middle" style="visibility: hidden" fill= 'white'></text>
        </g>
    </g>
</svg>

<Piano {noteValues}/>

<hr />
<form id="tuningForm">
    <h2>Tuning (by string Guitar/Bass): </h2>
    <label for="6th String">6/4:</label>
    <select id="6th String" name="6th String" bind:value={tuningNotes["6th String"]}>
        <option value="A">A</option>
        <option value="A#/Bb">A#/Bb</option>
        <option value="B">B</option>
        <option value="C">C</option>
        <option value="C#/Db">C#/Db</option>
        <option value="D">D</option>
        <option value="D#/Eb">D#/Eb</option>
        <option value="E">E</option>
        <option value="F">F</option>
        <option value="F#/Gb">F#/Gb</option>
        <option value="G">G</option>
        <option value="G#/Ab">G#/Ab</option>
    </select>
    <label for="5th String">5/3:</label>
    <select id="5th String" name="5th String" bind:value={tuningNotes["5th String"]}>
        <option value="A">A</option>
        <option value="A#/Bb">A#/Bb</option>
        <option value="B">B</option>
        <option value="C">C</option>
        <option value="C#/Db">C#/Db</option>
        <option value="D">D</option>
        <option value="D#/Eb">D#/Eb</option>
        <option value="E">E</option>
        <option value="F">F</option>
        <option value="F#/Gb">F#/Gb</option>
        <option value="G">G</option>
        <option value="G#/Ab">G#/Ab</option>
    </select>
    <label for="4th String">4/2:</label>
    <select id="4th String" name="4th String" bind:value={tuningNotes["4th String"]}>
        <option value="A">A</option>
        <option value="A#/Bb">A#/Bb</option>
        <option value="B">B</option>
        <option value="C">C</option>
        <option value="C#/Db">C#/Db</option>
        <option value="D">D</option>
        <option value="D#/Eb">D#/Eb</option>
        <option value="E">E</option>
        <option value="F">F</option>
        <option value="F#/Gb">F#/Gb</option>
        <option value="G">G</option>
        <option value="G#/Ab">G#/Ab</option>
    </select>
    <br />
    <br />
    <label for="3rd String">3/1:</label>
    <select id="3rd String" name="3rd String" bind:value={tuningNotes["3rd String"]}>
        <option value="A">A</option>
        <option value="A#/Bb">A#/Bb</option>
        <option value="B">B</option>
        <option value="C">C</option>
        <option value="C#/Db">C#/Db</option>
        <option value="D">D</option>
        <option value="D#/Eb">D#/Eb</option>
        <option value="E">E</option>
        <option value="F">F</option>
        <option value="F#/Gb">F#/Gb</option>
        <option value="G">G</option>
        <option value="G#/Ab">G#/Ab</option>
    </select>
    <label for="2nd String">2:</label>
    <select id="2nd String" name="2nd String" bind:value={tuningNotes["2nd String"]}>
        <option value="A">A</option>
        <option value="A#/Bb">A#/Bb</option>
        <option value="B">B</option>
        <option value="C">C</option>
        <option value="C#/Db">C#/Db</option>
        <option value="D">D</option>
        <option value="D#/Eb">D#/Eb</option>
        <option value="E">E</option>
        <option value="F">F</option>
        <option value="F#/Gb">F#/Gb</option>
        <option value="G">G</option>
        <option value="G#/Ab">G#/Ab</option>
    </select>
    <label for="1st String">1:</label>
    <select id="1st String" name="1st String" bind:value={tuningNotes["1st String"]}>
        <option value="A">A</option>
        <option value="A#/Bb">A#/Bb</option>
        <option value="B">B</option>
        <option value="C">C</option>
        <option value="C#/Db">C#/Db</option>
        <option value="D">D</option>
        <option value="D#/Eb">D#/Eb</option>
        <option value="E">E</option>
        <option value="F">F</option>
        <option value="F#/Gb">F#/Gb</option>
        <option value="G">G</option>
        <option value="G#/Ab">G#/Ab</option>
    </select>
</form>
<form id="ChordForm" on:submit|preventDefault={formSubmit}>
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
        <option value="power ">Power Chord</option>
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

    <div>
        <input type="radio" name="instrument" value="guitar" checked bind:group={instrument}/>
        <label for="guitar">Guitar</label>
        <input type="radio" name="instrument" value="bass" bind:group={instrument}/>
        <label for="bass">Bass</label>          
    </div>  

    <div>
        <input type="radio" name="guitar type" value="dots" checked bind:group={guitarType}/>
        <label for="dots">Dots</label>
        <input type="radio" name="guitar type" value="trapezoid" bind:group={guitarType}/>
        <label for="trapezoid">Trapezoid</label>      
        <input type="radio" name="guitar type" value="sharkFin" bind:group={guitarType}/>
        <label for="sharkFin">Shark Fin</label>     
        <input type="radio" name="guitar type" value="sideDots" bind:group={guitarType}/>
        <label for="sideDots">Side Dots</label>        
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
        font-size: 10px;
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
