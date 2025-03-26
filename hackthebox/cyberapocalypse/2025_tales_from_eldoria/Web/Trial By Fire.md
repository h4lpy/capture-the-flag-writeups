# Trial By Fire

As you ascend the treacherous slopes of the Flame Peaks, the scorching heat and shifting volcanic terrain test your endurance with every step. Rivers of molten lava carve fiery paths through the mountains, illuminating the night with an eerie crimson glow. The air is thick with ash, and the distant rumble of the earth warns of the danger that lies ahead. At the heart of this infernal landscape, a colossal Fire Drake awaits—a guardian of flame and fury, determined to judge those who dare trespass. With eyes like embers and scales hardened by centuries of heat, the Fire Drake does not attack blindly. Instead, it weaves illusions of fear, manifesting your deepest doubts and past failures. To reach the Emberstone, the legendary artifact hidden beyond its lair, you must prove your resilience, defying both the drake’s scorching onslaught and the mental trials it conjures. Stand firm, outwit its trickery, and strike with precision—only those with unyielding courage and strategic mastery will endure the Trial by Fire and claim their place among the legends of Eldoria.

-----

Navigating to the website, we see an input form to supply our name which has a limit of 30 characters:

![[Pasted image 20250322163541.png]]

Clicking through:

![[Pasted image 20250322163713.png]]

![[Pasted image 20250322163723.png]]

Reviewing the provided sourcecode, we see a hidden cheatcode within `flamedrake.html`, allowing us to enable a hidden button:

```javascript
import { DragonGame } from '/static/js/game.js';
import { addVisualEffects } from '/static/js/effects.js';

const game = new DragonGame();
game.init();
addVisualEffects(game);



// Konami code sequence to reveal the leet (Ancient Capture Device) button
// const konamiCode = ['ArrowUp', 'ArrowUp', 'ArrowDown', 'ArrowDown', 'ArrowLeft', 'ArrowRight', 'ArrowLeft', 'ArrowRight', 'b', 'a'];
const konamiCode = ['ArrowUp'];
let konamiIndex = 0;

document.addEventListener('keydown', (e) => {
  if (e.key === konamiCode[konamiIndex]) {
	konamiIndex++;
	if (konamiIndex === konamiCode.length) {
	  document.querySelector('.leet').classList.remove('hidden');
	  konamiIndex = 0;
	}
  } else {
	konamiIndex = 0;
  }
});
```

![[Pasted image 20250322163941.png]]

Results in the following hint indicating Server-Side Template Injection is the intended path:

![[Pasted image 20250322164019.png]]

