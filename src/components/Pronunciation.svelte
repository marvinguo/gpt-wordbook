<script lang="ts">
  export let word: string;
  export let uk: string = '';
  export let us: string = '';

  let isPlayingUK = false;
  let isPlayingUS = false;

  const playPronunciation = (accent: 'uk' | 'us') => {
    if ('speechSynthesis' in window) {
      window.speechSynthesis.cancel();
      
      const utterance = new SpeechSynthesisUtterance(word);
      const voices = window.speechSynthesis.getVoices();
      const targetVoice = voices.find(voice => 
        accent === 'uk' 
          ? voice.lang.startsWith('en-GB')
          : voice.lang.startsWith('en-US')
      );
      
      if (targetVoice) {
        utterance.voice = targetVoice;
      }
      utterance.lang = accent === 'uk' ? 'en-GB' : 'en-US';
      utterance.rate = 0.8;
      
      if (accent === 'uk') {
        isPlayingUK = true;
        utterance.onend = () => { isPlayingUK = false; };
      } else {
        isPlayingUS = true;
        utterance.onend = () => { isPlayingUS = false; };
      }
      
      window.speechSynthesis.speak(utterance);
    }
  };

  if ('speechSynthesis' in window) {
    window.speechSynthesis.getVoices();
    window.speechSynthesis.onvoiceschanged = () => {
      window.speechSynthesis.getVoices();
    };
  }
</script>

{#if uk || us}
  <div class="mx-auto !mt-8 mb-8 max-w-fit [perspective:1000px] print:hidden">
    <div class="flex items-center p-[0.4rem] bg-white/60 [:root[data-theme='dark']_&]:bg-[#1c1c1e]/60 backdrop-blur-[20px] backdrop-saturate-[180%] border border-black/[0.08] [:root[data-theme='dark']_&]:border-white/0.08 rounded-[1.5rem] shadow-[0_10px_30px_-5px_rgba(0,0,0,0.05),0_4px_10px_-2px_rgba(0,0,0,0.02),inset_0_1px_1px_rgba(255,255,255,0.8)] [:root[data-theme='dark']_&]:shadow-[0_15px_35px_-5px_rgba(0,0,0,0.3),0_5px_15px_-2px_rgba(0,0,0,0.1),inset_0_1px_0_rgba(255,255,255,0.05)] transition-all duration-500">
      
      {#if uk}
        <button 
          class="group flex flex-row items-center justify-center gap-3 py-[0.6rem] px-[1.2rem] bg-transparent border-none rounded-[1.2rem] cursor-pointer text-[#1d1d1f] [:root[data-theme='dark']_&]:text-[#f5f5f7] transition-all duration-300 hover:bg-black/[0.03] [:root[data-theme='dark']_&]:hover:bg-white/[0.05] active:scale-[0.96] active:bg-black/[0.06] {isPlayingUK ? 'text-[#8b5cf6]' : ''}"
          on:click={() => playPronunciation('uk')}
          aria-label="播放英式发音"
          title="播放英式发音"
        >
          <span class="text-[0.7rem] font-bold opacity-50 tracking-wider uppercase">UK</span>
          <span class="font-sans text-[0.95rem] tracking-wide opacity-90">{uk}</span>
          <div class="relative w-[1.1rem] h-[1.1rem] flex items-center justify-center">
             {#if isPlayingUK}
               <span class="absolute inset-0 rounded-full bg-[#8b5cf6]/20 animate-ping"></span>
             {/if}
             <svg class="w-full h-full opacity-60 group-hover:opacity-100 transition-opacity" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
               <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon>
               <path d="M15.54 8.46a5 5 0 0 1 0 7.07"></path>
             </svg>
          </div>
        </button>
      {/if}

      {#if uk && us}
        <div class="w-[1px] h-6 bg-black/[0.08] [:root[data-theme='dark']_&]:bg-white/0.1 !mx-6 !my-0"></div>
      {/if}

      {#if us}
        <button 
          class="!mt-0 group flex flex-row items-center justify-center gap-3 py-[0.6rem] px-[1.2rem] bg-transparent border-none rounded-[1.2rem] cursor-pointer text-[#1d1d1f] [:root[data-theme='dark']_&]:text-[#f5f5f7] transition-all duration-300 hover:bg-black/[0.03] [:root[data-theme='dark']_&]:hover:bg-white/[0.05] active:scale-[0.96] active:bg-black/[0.06] {isPlayingUS ? 'text-[#8b5cf6]' : ''}"
          on:click={() => playPronunciation('us')}
          aria-label="播放美式发音"
          title="播放美式发音"
        >
          <span class="text-[0.7rem] font-bold opacity-50 tracking-wider uppercase">US</span>
          <span class="font-sans text-[0.95rem] tracking-wide opacity-90">{us}</span>
          <div class="relative w-[1.1rem] h-[1.1rem] flex items-center justify-center">
             {#if isPlayingUS}
               <span class="absolute inset-0 rounded-full bg-[#8b5cf6]/20 animate-ping"></span>
             {/if}
             <svg class="w-full h-full opacity-60 group-hover:opacity-100 transition-opacity" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
               <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon>
               <path d="M15.54 8.46a5 5 0 0 1 0 7.07"></path>
             </svg>
          </div>
        </button>
      {/if}

    </div>
  </div>
{/if}
