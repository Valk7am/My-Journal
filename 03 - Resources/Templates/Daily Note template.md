---
banner: 03 - Resources/Attachements/wallhaven_banner2.jpg
exercise: 8 hours
aliases:
tags:
cssclasses:
---
أصبحنا وأصبح الملك لله، والحمد لله، لا إله إلا الله وحده لا شريك له، له الملك وله الحمد وهو على كل شيء قدير

َعَنْ زَيْدِ بنِ أَرْقَم رضي الله عنه، قَالَ: كَانَ رَسُولُ اللَّه ﷺ يقَولُ:
اللهُمَّ إِنِّي أَعُوذُ بِكَ مِنَ العَجْزِ وَالكَسَلِ، والبُخْلِ وَالهَرم، وعَذَاب الْقَبْر، اللَّهُمَّ آتِ نَفْسِي تَقْوَاهَا، وَزَكِّهَا أَنْتَ خَيرُ مَنْ زَكَّاهَا، أَنْتَ ولِيُّهَا وَموْلاَهَا، اللَّهُمَّ إِنِّي أَعُوذُ بِكَ مِنْ عِلمٍ لا يَنْفَعُ، ومِنْ قَلْبٍ لاَ يخْشَعُ، وَمِنْ نَفْسٍ لاَ تَشبَعُ، ومِنْ دَعْوةٍ لا يُسْتجابُ لهَا

وَعنِ ابنِ عبَّاسٍ رَضِيَ اللَّه عَنْهُمَا، أَنَّ رَسُولَ اللَّهِ ﷺ كَانَ يَقُولُ:
اللَّهُمَّ لَكَ أَسْلَمْتُ، وَبِكَ آمَنْتُ، وعلَيْكَ تَوَكَّلْتُ، وَإِلَيْكَ أَنَبْتُ وَبِكَ خَاصَمْتُ، وإِلَيْكَ حَاكَمْتُ. فاغْفِرْ لِي مَا قَدَّمْتُ، وَمَا أَخَّرْتُ، وَمَا أَسْررْتُ ومَا أَعلَنْتُ، أَنْتَ المُقَدِّمُ، وَأَنْتَ المُؤَخِّرُ، لا إِلَهَ إِلاَّ أَنْتَ.



> <font color="#fac08f">Tasks</font>

> <font color="#fac08f">Tasks</font> this week

<%*
// ----------------------------
// CONFIG
// ----------------------------
const vault = app.vault;
const dailyFolder = "01 - Daily"; // folder where daily notes live
const today = window.moment();
const startOfWeek = today.clone().day(0); // Sunday = start of week

const startMarker = '> ' + '<font color="#fac08f">Tasks</font>';
const endMarker   = '> ' + '<font color="#fac08f">Tasks</font> this week';

// ----------------------------
// CREATE / MOVE CURRENT NOTE (your working block logic)
// ----------------------------
const creationDate = tp.file.creation_date("YYYY/MMMM");
const targetFolder = `${dailyFolder}/${creationDate}`;
const targetPath = `${targetFolder}/${tp.file.title}`;

// Ensure folder exists
if (!vault.getAbstractFileByPath(targetFolder)) {
    await vault.createFolder(targetFolder);
}

const originalFile = app.workspace.getActiveFile();
const existingFile = tp.file.find_tfile(targetPath);

// If file already exists, open it and delete the original
if (existingFile) {
    await app.workspace.getLeaf(true).openFile(existingFile);
    await new Promise(r => setTimeout(r, 500));  
    await vault.trash(originalFile, true);
    return;
} else {
    try {
        await tp.file.move(targetPath);
    } catch(e) {
        console.log("Move failed:", e);
    }
}

// ----------------------------
// FIND CLOSEST TASKS BLOCK
// ----------------------------
let output = "";
let found = false;
let current = today.clone().subtract(1, 'days');

while(current.isSameOrAfter(startOfWeek)) {
    const date = current.format("DD - dddd");
    const path = `${targetFolder}/${date}.md`;
    const file = vault.getAbstractFileByPath(path);
    if(file) {
        const content = await vault.read(file);
        const start = content.indexOf(startMarker);
        const end = content.indexOf(endMarker);
        if(start !== -1 && end !== -1 && end > start) {
            const text = content.slice(start + startMarker.length, end).trim();
            if(text.length > 0) {
                output = `**${date}\n ${text}**`;
                found = true;
                break;
            }
        }
    }
    current.subtract(1, 'days');
}

// ----------------------------
// OUTPUT TO NOTE
// ----------------------------
tR += found ? output : "";
%>

> Extras:


> References

<%*

// --- Ensure folder exists ---
if (!vault.getAbstractFileByPath(targetFolder)) {
    await vault.createFolder(targetFolder);
}

// --- If file exists AND it's not the same file ---
if (existingFile ) {

	// Open existing file (correct API)  
	await app.workspace.getLeaf(true).openFile(existingFile);  
	  
	// 2. Wait for Obsidian to switch context  
	await new Promise(r => setTimeout(r, 500));  
	  
	// 3. Delete original  
	await vault.trash(originalFile, true);
		
	// ❗ 4. HARD STOP (very important)  
	return;
} else {
    try {
        await tp.file.move(targetPath);
    } catch (e) {
        console.log("Move failed:", e);
    }
}
%>