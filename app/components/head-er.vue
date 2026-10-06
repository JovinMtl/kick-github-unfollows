<template>
    <nav class="bg-[#0d0d0d] px-6 py-4 flex items-center justify-between">
      <!-- Logo -->
      <div class="flex items-center gap-3">
        <!-- Terminal icon (mobile only) -->
        <div class="w-9 h-9 border border-[#5dd8be] rounded flex items-center justify-center">
          <span class="text-[#5dd8be] text-xs font-bold" style="font-size: 1.5rem;">K</span>
        </div>
        <span class="text-[#5dd8be] font-bold text-sm tracking-tight">kick-github-unfollows</span>
      </div>

      <!-- Desktop nav links -->
      <div class="hidden md:flex items-center gap-8">
        <a href="#" class="text-[#5dd8be] font-bold text-sm tracking-wide">dashboard</a>
        <a href="#" class="text-[#4a7a72] text-sm tracking-wide hover:text-[#5dd8be] transition-colors">history</a>
        <a href="#" class="text-[#4a7a72] text-sm tracking-wide hover:text-[#5dd8be] transition-colors">settings</a>
        <a href="#" class="text-[#4a7a72] text-sm tracking-wide hover:text-[#5dd8be] transition-colors">about</a>
      </div>

      <!-- Mobile hamburger -->
      <button class="md:hidden w-9 h-9 border border-[#5dd8be] rounded flex flex-col items-center justify-center gap-1.5">
        <span class="block w-4 h-px bg-[#5dd8be]"></span>
        <span class="block w-4 h-px bg-[#5dd8be]"></span>
        <span class="block w-4 h-px bg-[#5dd8be]"></span>
      </button>
    </nav>

    <!-- Mobile tab bar -->
    <div class="md:hidden bg-[#1a1a1a] flex items-center border-b border-[#2a2a2a]">
      <a href="#" class="px-4 py-3 text-[#0d0d0d] bg-[#5dd8be] text-xs font-bold tracking-wide rounded-sm mx-2 my-2">dashboard</a>
      <a href="#" class="px-4 py-3 text-[#4a7a72] text-xs tracking-wide">history</a>
      <a href="#" class="px-4 py-3 text-[#4a7a72] text-xs tracking-wide">settings</a>
      <a href="#" class="px-4 py-3 text-[#4a7a72] text-xs tracking-wide">about</a>
    </div>
</template>