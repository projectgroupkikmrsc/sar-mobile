<template>
  <div style="font-family: 'Segoe UI', Roboto, sans-serif; background-color: #0f172a; min-height: 100vh; color: white; display: flex; flex-direction: column; overflow-x: hidden;">
    
    <div v-if="currentScreen === 'welcome'" 
         :style="{ 
           backgroundImage: `linear-gradient(rgba(15, 23, 42, 0.75), rgba(15, 23, 42, 0.75)), url(${heliBg})`, 
           backgroundSize: 'cover', 
           backgroundPosition: 'center' 
         }"
         style="flex: 1; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 30px;"> 
      <div style="text-align: center; max-width: 400px; width: 100%; margin-top: -40px;">
        <div style="width: 100px; height: 100px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin: 0 auto 15px auto; filter: drop-shadow(0 0 15px rgba(37,99,235,0.3));">
          <img :src="logoApmm" alt="Logo APMM" style="width: 100%; height: 100%; object-fit: contain;" />
        </div>
        
        <div style="margin-bottom: 35px;">
          <h1 class="wave-text" style="margin: 0; font-size: 20px; letter-spacing: 2px; font-weight: 900; line-height: 1.3; color: #ffffff; text-transform: uppercase;">
            MARITIME SEARCH AND RESCUE UNIT NAVIGATOR
          </h1>
          <div style="font-size: 11px; color: #38bdf8; font-weight: 800; letter-spacing: 1px; margin-top: 10px; text-transform: uppercase;">
            BAHAGIAN CARILAMAT DAN BANTUAN BENCANA
          </div>
          <div style="font-size: 10px; color: #94a3b8; font-weight: 700; letter-spacing: 1px; margin-top: 4px; text-transform: uppercase;">
            AGENSI PENGUATKUASAAN MARITIM MALAYSIA
          </div>
        </div>

        <div v-if="!otpDihantar" class="fade-in-content" style="background: #1e293b; padding: 25px; border-radius: 12px; border: 1px solid #334155; box-shadow: 0 10px 25px rgba(0,0,0,0.3); text-align: center;">
          <label style="display: block; font-size: 11px; font-weight: bold; color: #f8fafc; margin-bottom: 10px; text-transform: uppercase; letter-spacing: 0.5px;">MASUKKAN NOMBOR TELEFON</label>
          <input type="tel" v-model="noTelefon" placeholder="+601XXXXXXXX" style="width: 100%; box-sizing: border-box; padding: 12px; background: #0f172a; border: 1px solid #475569; border-radius: 6px; color: white; font-size: 15px; text-align: center; margin-bottom: 15px; font-weight: bold; letter-spacing: 1px;" />
          
          <div style="display: flex; justify-content: center;">
            <button @click="hantarSmsOtp" :disabled="!noTelefon" style="background: #10b981; color: white; border: none; padding: 10px 24px; font-size: 12px; font-weight: 600; border-radius: 25px; cursor: pointer; transition: 0.3s; display: flex; align-items: center; gap: 8px; box-shadow: 0 4px 12px rgba(16,185,129,0.3);">
              Dapatkan Kod OTP
            </button>
          </div>
        </div>

        <div v-else class="fade-in-content" style="background: #1e293b; padding: 25px; border-radius: 12px; border: 1px solid #334155; box-shadow: 0 10px 25px rgba(0,0,0,0.3); text-align: left;">
          <div style="background: rgba(52,211,153,0.1); border: 1px solid #34d399; padding: 10px; border-radius: 6px; color: #34d399; font-size: 12px; margin-bottom: 15px; text-align: center; font-weight: 500;">
            Simulasi SMS: Kod pengesahan anda adalah <strong style="font-size: 14px; letter-spacing: 1px;">{{ simulasiOtp }}</strong>
          </div>
          
          <label style="display: block; font-size: 10px; font-weight: bold; color: #94a3b8; margin-bottom: 8px; text-transform: uppercase;">MASUKKAN KOD KESELAMATAN 4-DIGIT</label>
          <input type="text" v-model="inputOtp" maxlength="4" placeholder="0 0 0 0" style="width: 100%; box-sizing: border-box; padding: 12px; background: #0f172a; border: 1px solid #475569; border-radius: 6px; color: #38bdf8; font-size: 22px; text-align: center; letter-spacing: 8px; font-family: monospace; margin-bottom: 15px; font-weight: bold;" />
          
          <button @click="sahkanSmsOtp" :disabled="inputOtp.length !== 4" style="width: 100%; background: #10b981; color: white; border: none; padding: 14px; font-size: 14px; font-weight: bold; border-radius: 6px; cursor: pointer;">
            CONFIRM
          </button>
          
          <button @click="otpDihantar = false" style="width: 100%; background: transparent; color: #94a3b8; border: none; margin-top: 10px; font-size: 12px; cursor: pointer; text-decoration: underline;">
            Kembali
          </button>
        </div>
      </div>
    </div>

    <div v-else-if="currentScreen === 'setup'" 
         :style="{ 
           backgroundImage: `linear-gradient(rgba(15, 23, 42, 0.85), rgba(15, 23, 42, 0.85)), url(${heliBg})`, 
           backgroundSize: 'cover', 
           backgroundPosition: 'center' 
         }"
         style="flex: 1; display: flex; flex-direction: column; padding: 40px 20px 20px 20px; justify-content: space-between;">
      <div class="fade-in-content">
        <div style="text-align: center; margin-bottom: 22px;">
          <div style="display: inline-flex; align-items: center; gap: 6px; background: rgba(30, 41, 59, 0.8); border: 1px solid #334155; border-radius: 20px; padding: 4px 14px; margin-bottom: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.3);">
            <span style="font-size: 12px;">⚓</span>
            <span style="font-size: 10px; font-weight: 800; color: #38bdf8; letter-spacing: 1.5px; text-transform: uppercase;">MARITIME SRU NAVIGATOR</span>
          </div>
          <h2 style="margin: 0; font-size: 22px; font-weight: bold; color: #f8fafc;">Mission Setup</h2>
          <p style="margin: 4px 0 0 0; font-size: 11px; color: #94a3b8;">Verifikasi bidang kuasa MRSC dan aset operasi sebelum memuat turun pelan SAROPS.</p>
        </div>

        <div style="display: flex; flex-direction: column; gap: 16px;">
          <div>
            <label style="display: block; font-size: 11px; font-weight: bold; color: #94a3b8; margin-bottom: 6px; letter-spacing: 0.5px;">MARITIME RESCUE SUB-CENTER (MRSC)</label>
            <select v-model="selectedMrsc" @change="kendalikanTukarMrsc" style="width: 100%; padding: 12px; background: #1e293b; border: 1px solid #334155; border-radius: 6px; color: white; font-size: 14px; font-weight: bold; outline: none;">
              <option value="" disabled>-- PILIH JURISDIKSI MRSC --</option>
              <option value="MRSC Langkawi">MRSC LANGKAWI (Wilayah Utara)</option>
              <option value="MRSC Klang">MRSC KLANG (Wilayah Barat)</option>
              <option value="MRSC Johor Baharu">MRSC JOHOR BAHARU (Wilayah Selatan)</option>
              <option value="MRSC Kuantan">MRSC KUANTAN (Wilayah Timur)</option>
              <option value="MRSC Kota Kinabalu">MRSC KOTA KINABALU (Wilayah Sabah)</option>
              <option value="MRSC Kuching">MRSC KUCHING (Wilayah Sarawak)</option>
            </select>
          </div>

          <div>
            <label style="display: block; font-size: 11px; font-weight: bold; color: #94a3b8; margin-bottom: 6px; letter-spacing: 0.5px;">ACTIVE SAR INCIDENT</label>
            <select v-model="selectedCaseId" :disabled="!selectedMrsc" @change="kendalikanTukarKes" style="width: 100%; padding: 12px; background: #1e293b; border: 1px solid #334155; border-radius: 6px; color: white; font-size: 14px; font-weight: bold; outline: none;" :style="!selectedMrsc ? 'opacity: 0.5;' : ''">
              <option value="" disabled>-- {{ selectedMrsc ? 'PILIH INSIDEN AKTIF' : 'SILA PILIH MRSC DAHULU' }} --</option>
              <option v-for="kes in senaraiKesTapis" :key="kes.id" :value="kes.id">
                #{{ kes.id }} - {{ kes.case_name }}
              </option>
            </select>
            <small v-if="selectedMrsc && senaraiKesTapis.length === 0" style="color: #f59e0b; font-size: 11px; display: block; margin-top: 5px;">⚠️ Tiada kes aktif didaftarkan oleh SMC di stesen ini.</small>
          </div>

          <div>
            <label style="display: block; font-size: 11px; font-weight: bold; color: #94a3b8; margin-bottom: 6px; letter-spacing: 0.5px;">BOAT / ASSET ID</label>
            <select v-model="selectedAsset" :disabled="!selectedCaseId" style="width: 100%; padding: 12px; background: #1e293b; border: 1px solid #334155; border-radius: 6px; color: white; font-size: 14px; font-weight: bold; outline: none;" :style="!selectedCaseId ? 'opacity: 0.5;' : ''">
              <option value="" disabled>-- {{ selectedCaseId ? 'PILIH ASET OPERASI' : 'SILA PILIH INSIDEN DAHULU' }} --</option>
              <option v-for="aset in senaraiAsetTapis" :key="aset.id" :value="aset.sru_name" :disabled="aset.isTaken" :style="{ color: aset.isMySession ? '#60a5fa' : (aset.isTaken ? '#f87171' : '#4ade80'), backgroundColor: '#1e293b' }">
                {{ aset.isMySession ? '🔵' : (aset.isTaken ? '🔴' : '🟢') }} {{ aset.sru_name }} {{ aset.isMySession ? '(SESI ANDA - TEKAN UNTUK SAMBUNG)' : (aset.isTaken ? '(SEDANG DIGUNAKAN PERANTI LAIN)' : '(READY)') }}
              </option>
            </select>
            <small v-if="selectedCaseId && senaraiAsetTapis.length === 0" style="color: #f59e0b; font-size: 11px; display: block; margin-top: 5px;">⚠️ SMC belum memuat naik atau memplot pelan SRU bagi kes ini di web.</small>
          </div>
        </div>
      </div>

      <button @click="mulaSync" :disabled="!selectedMrsc || !selectedCaseId || !selectedAsset" class="fade-in-content" style="width: 100%; padding: 15px; border: none; border-radius: 6px; font-weight: bold; font-size: 16px; text-transform: uppercase; letter-spacing: 0.5px; transition: 0.3s; margin-top: 0px; cursor: pointer;" :style="(!selectedMrsc || !selectedCaseId || !selectedAsset) ? 'background-color: #334155; color: #64748b; cursor: not-allowed;' : 'background-color: #2563eb; color: white; box-shadow: 0 4px 14px rgba(37,99,235,0.4);'">
        Connect & Sync Data
      </button>
    </div>

    <div v-else-if="currentScreen === 'map'" 
         class="fade-in-content"
         :class="{ 'night-vision': isNightMode }"
         :style="{ 
           backgroundImage: `linear-gradient(rgba(15, 23, 42, 0.85), rgba(15, 23, 42, 0.85)), url(${heliBg})`, 
           backgroundSize: 'cover', 
           backgroundPosition: 'center' 
         }"
         style="height: 100vh; display: flex; flex-direction: column; position: relative;">
      
      <!-- Banner Notifikasi Terapung (Mesej / Sighting / MOB dari peranti lain) -->
      <transition name="pop">
        <div v-if="toastNotifikasi.papar" 
             @click="klikToastNotifikasi"
             :style="{
               background: toastNotifikasi.jenis === 'mob' ? 'rgba(220, 38, 38, 0.96)' : (toastNotifikasi.jenis === 'sighting' ? 'rgba(234, 88, 12, 0.96)' : 'rgba(15, 23, 42, 0.96)'),
               borderColor: toastNotifikasi.jenis === 'mob' ? '#ffffff' : (toastNotifikasi.jenis === 'sighting' ? '#fb923c' : '#38bdf8')
             }"
             style="position: absolute; top: 12px; left: 12px; right: 12px; z-index: 10000; border-width: 1.5px; border-style: solid; border-radius: 8px; padding: 10px 14px; display: flex; align-items: center; justify-content: space-between; gap: 10px; box-shadow: 0 8px 30px rgba(0,0,0,0.8); cursor: pointer; backdrop-filter: blur(10px); animation: pulse 1.5s infinite;">
          <div style="display: flex; align-items: center; gap: 10px; flex: 1; min-width: 0;">
            <span style="font-size: 22px;">{{ toastNotifikasi.ikon }}</span>
            <div style="flex: 1; min-width: 0;">
              <div style="font-size: 11px; font-weight: 800; color: #ffffff; text-transform: uppercase; letter-spacing: 0.5px;">{{ toastNotifikasi.tajuk }}</div>
              <div style="font-size: 10px; color: #f1f5f9; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; margin-top: 1px;">{{ toastNotifikasi.mesej }}</div>
            </div>
          </div>
          <button @click.stop="toastNotifikasi.papar = false" style="background: rgba(0,0,0,0.3); border: none; color: white; width: 22px; height: 22px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 11px; cursor: pointer;">✕</button>
        </div>
      </transition>

      <div style="position: absolute; top: 10px; left: 10px; right: 10px; z-index: 999; display: flex; flex-direction: column; gap: 6px;">
        <!-- Bar 1: Data Telemetri & Navigasi Waypoint -->
        <div style="background: rgba(15, 23, 42, 0.7); backdrop-filter: blur(10px); border: 1px solid #334155; border-radius: 8px; padding: 8px; display: flex; gap: 4px; box-shadow: 0 4px 15px rgba(0,0,0,0.5);">
          <!-- SPEED -->
          <div style="flex: 1; background: rgba(15, 23, 42, 0.4); padding: 6px; border-radius: 6px; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 40px;">
            <div style="font-size: 8px; color: #94a3b8; font-weight: bold; text-transform: uppercase;">SPEED</div>
            <div style="font-size: 13px; color: #00ffff; font-family: monospace; font-weight: bold;">{{ botSpeed.toFixed(1) }} kts</div>
          </div>
          <!-- HDG -->
          <div style="flex: 1; background: rgba(15, 23, 42, 0.4); padding: 6px; border-radius: 6px; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 40px;">
            <div style="font-size: 8px; color: #94a3b8; font-weight: bold; text-transform: uppercase;">HDG</div>
            <div style="font-size: 13px; color: #ffffff; font-family: monospace; font-weight: bold;">{{ botHeading !== '-' ? Number(botHeading).toFixed(1) : '-' }}°</div>
          </div>
          
          <!-- JIKA MISI CARIAN SEDANG BERJALAN -->
          <template v-if="!isSearchCompleted && (activeCspCoord || activeSortieWaypoints.length > 0)">
            <!-- TARGET BOX -->
            <div style="flex: 1.1; background: rgba(15, 23, 42, 0.4); padding: 6px; border-radius: 6px; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 40px;">
              <div style="font-size: 8px; color: #94a3b8; font-weight: bold; text-transform: uppercase;">TARGET</div>
              <div style="font-size: 11px; color: #ffffff; font-family: monospace; font-weight: bold; white-space: nowrap;">{{ activeTargetLabel }}</div>
            </div>

            <!-- COURSE BOX -->
            <div style="flex: 1; background: rgba(15, 23, 42, 0.4); padding: 6px; border-radius: 6px; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 40px;">
              <div style="font-size: 8px; color: #94a3b8; font-weight: bold; text-transform: uppercase;">COURSE</div>
              <div style="font-size: 13px; color: #fbbf24; font-family: monospace; font-weight: bold;">{{ courseToTarget }}°</div>
            </div>
            
            <!-- ETA Box -->
            <div style="flex: 1; background: rgba(15, 23, 42, 0.4); padding: 6px; border-radius: 6px; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 40px;">
              <div style="font-size: 8px; color: #94a3b8; font-weight: bold; text-transform: uppercase;">⏳ ETA</div>
              <div style="font-size: 11px; font-weight: bold; color: #fbbf24; font-family: monospace; white-space: nowrap;">{{ computedETA }}</div>
            </div>
            
            <!-- Distance Box -->
            <div style="flex: 1; background: rgba(15, 23, 42, 0.4); padding: 6px; border-radius: 6px; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 40px;">
              <div style="font-size: 8px; color: #94a3b8; font-weight: bold; text-transform: uppercase;">DIST</div>
              <div style="font-size: 11px; font-weight: bold; color: #4ade80; font-family: monospace; white-space: nowrap;">{{ distanceToTarget }} NM</div>
            </div>
          </template>

          <!-- JIKA SEMUA WAYPOINT SELESAI -->
          <div v-else-if="isSearchCompleted" style="flex: 4; text-align: center; display: flex; align-items: center; justify-content: center; min-height: 40px; background: rgba(16, 185, 129, 0.2); border: 1px solid #10b981; border-radius: 6px;">
             <div style="font-size: 12px; color: #34d399; font-weight: bold; animation: pulse 2s infinite;">🏁 CARIAN SELESAI (100%)</div>
          </div>

          <!-- JIKA TIADA PELAN / MENUNGGU DATA -->
          <div v-else style="flex: 4; text-align: center; display: flex; align-items: center; justify-content: center; min-height: 40px; background: rgba(15, 23, 42, 0.4); border-radius: 6px;">
             <div style="font-size: 11px; color: #94a3b8; font-weight: bold;">MENUNGGU PELAN SAR...</div>
          </div>
        </div>

        <!-- Bar Kemajuan Keseluruhan (% Selesai) -->
        <div v-if="activeCspCoord || activeSortieWaypoints.length > 0" style="background: rgba(15, 23, 42, 0.8); border: 1px solid #334155; border-radius: 6px; padding: 4px 8px; display: flex; flex-direction: column; gap: 3px; box-shadow: 0 4px 15px rgba(0,0,0,0.5);">
          <div style="display: flex; justify-content: space-between; align-items: center; font-size: 9px;">
            <span style="color: #94a3b8; font-weight: bold;">
              KEMAJUAN: <span style="color: #4ade80;">{{ completedWpCount }} SELESAI</span>
              <span v-if="skippedWpCount > 0" style="color: #f59e0b; margin-left: 4px;">• {{ skippedWpCount }} DILANGKAU</span>
              <span style="color: #94a3b8; margin-left: 4px;">/ {{ totalTargetsCount }} TITIK</span>
            </span>
            <span style="color: #4ade80; font-weight: 900; font-family: monospace;">{{ progressPercentage }}% SIAP</span>
          </div>
          <div style="background: #1e293b; height: 5px; border-radius: 3px; overflow: hidden;">
            <div :style="{ width: progressPercentage + '%' }" style="background: linear-gradient(90deg, #3b82f6, #10b981); height: 100%; transition: width 0.5s ease-in-out; border-radius: 3px;"></div>
          </div>
        </div>

        <!-- Bar 2: Maklumat Aset & Kedudukan (Sebaris) -->
        <div style="background: rgba(15, 23, 42, 0.7); backdrop-filter: blur(10px); border: 1px solid #334155; border-radius: 8px; padding: 8px 12px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 4px 15px rgba(0,0,0,0.5); font-size: 11px;">
           <span>ASET: <strong style="color: #60a5fa;">{{ selectedAsset }}</strong></span>
           <span style="color: #cbd5e1; font-family: monospace; font-weight: bold;">{{ formatCoordinate(currentLat, true) }} {{ formatCoordinate(currentLng, false) }}</span>
           <span :style="{color: isTracking ? (isOnlineStatus ? '#4ade80' : '#f59e0b') : '#f87171'}" style="font-weight: 900; letter-spacing: 0.5px;">
             {{ isTracking ? (isOnlineStatus ? '● ONLINE' : `⚠️ OFFLINE (${offlineQueue.length})`) : '● OFFLINE' }}
           </span>
        </div>

        <!-- Bar 3: Floating Action Utility Column (Layer, Recenter, Weather, Night Mode, MOB) -->
        <div style="display: flex; flex-direction: column; align-items: flex-end; gap: 8px;">
           <!-- Tukar Mod Peta Satelit / Biasa -->
           <button @click="tukarModPeta" title="Tukar Mod Peta" style="background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(10px); border: 1px solid #334155; border-radius: 8px; padding: 6px; width: 38px; height: 38px; display: flex; justify-content: center; align-items: center; color: white; cursor: pointer; box-shadow: 0 4px 15px rgba(0,0,0,0.5);">
             <span v-if="isSatelit" style="font-size: 18px;">🗺️</span>
             <span v-else style="font-size: 18px;">🛰️</span>
           </button>
           
           <!-- Recenter GPS -->
           <button @click="recenterMap" title="Pusatkan Peta ke GPS" style="background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(10px); border: 1px solid #334155; border-radius: 8px; padding: 6px; width: 38px; height: 38px; display: flex; justify-content: center; align-items: center; color: #60a5fa; cursor: pointer; box-shadow: 0 4px 15px rgba(0,0,0,0.5);" :style="isFollowing ? 'color: #00ffff; border-color: #00ffff;' : ''">
             🎯
           </button>

           <!-- Cuaca Maritim & Laut (Live Marine Weather) -->
           <button @click="bukaModalCuaca" title="Cuaca & Keadaan Laut Maritim" style="background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(10px); border: 1px solid #334155; border-radius: 8px; padding: 6px; width: 38px; height: 38px; display: flex; justify-content: center; align-items: center; color: white; cursor: pointer; box-shadow: 0 4px 15px rgba(0,0,0,0.5);" :style="paparCuacaModal ? 'border-color: #38bdf8; background: rgba(56, 189, 248, 0.25);' : ''">
             <span style="font-size: 18px;">🌦️</span>
           </button>

           <!-- Mod Malam (Night Vision Red Mode) -->
           <button @click="toggleNightMode" title="Mod Malam (Night Vision)" style="background: rgba(15, 23, 42, 0.8); backdrop-filter: blur(10px); border: 1px solid #334155; border-radius: 8px; padding: 6px; width: 38px; height: 38px; display: flex; justify-content: center; align-items: center; cursor: pointer; box-shadow: 0 4px 15px rgba(0,0,0,0.5);" :style="isNightMode ? 'border-color: #ef4444; background: rgba(127, 29, 29, 0.5);' : ''">
             <span style="font-size: 18px;">{{ isNightMode ? '☀️' : '🌙' }}</span>
           </button>

           <!-- Butang Pantas SOS / MOB (Man Overboard) -->
           <button @click="paparMobModal = true; paparWaypointList = false; paparChat = false; paparSightingModal = false; paparConfirmStop = false; paparCuacaModal = false" title="Kecemasan MOB" style="background: rgba(220, 38, 38, 0.85); backdrop-filter: blur(10px); border: 1px solid #f87171; border-radius: 8px; padding: 6px; width: 38px; height: 38px; display: flex; flex-direction: column; justify-content: center; align-items: center; color: white; cursor: pointer; box-shadow: 0 4px 15px rgba(220, 38, 38, 0.6); animation: pulse 2s infinite;">
             <span style="font-size: 13px; font-weight: 900;">🆘</span>
             <span style="font-size: 7px; font-weight: 900; line-height: 1;">MOB</span>
           </button>
        </div>
      </div>

      <!-- Banner Amaran Jika MOB Aktif -->
      <div v-if="isMobActive" style="position: absolute; top: 180px; left: 10px; right: 60px; z-index: 9999; background: rgba(220, 38, 38, 0.95); border: 1px solid #ffffff; border-radius: 8px; padding: 8px 12px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 4px 20px rgba(220, 38, 38, 0.8); animation: pulse 1s infinite;">
        <div style="display: flex; align-items: center; gap: 8px;">
          <span style="font-size: 18px;">🚨</span>
          <div>
            <div style="font-size: 11px; font-weight: bold; color: white;">AMARAN KECEMASAN MOB AKTIF!</div>
            <div style="font-size: 10px; color: #fee2e2; font-family: monospace;">Jarak: {{ distanceToMob }} NM | Haluan: {{ courseToMob }}°</div>
          </div>
        </div>
        <button @click="paparMobModal = true" style="background: white; color: #dc2626; border: none; padding: 4px 8px; border-radius: 4px; font-size: 10px; font-weight: bold; cursor: pointer;">
          LIHAT
        </button>
      </div>

      <div id="mapContainer" style="flex-grow: 1; width: 100%; z-index: 1;"></div>

      <!-- Bottom Tactical Control Container -->
      <div style="position: absolute; bottom: 15px; left: 15px; right: 15px; padding: 12px; background-color: rgba(15, 23, 42, 0.75); backdrop-filter: blur(10px); border: 1px solid #334155; border-radius: 12px; z-index: 10; box-shadow: 0 5px 25px rgba(0,0,0,0.5); display: flex; flex-direction: column; gap: 10px;">
        <!-- Tactical Tapping Panels (Combined Transition for smooth switching) -->
        <transition name="slide-up" mode="out-in">
          <!-- 1. MISSION SAP PANEL (WITH REVERSE & SKIP WP & MANUAL SELECT) -->
          <div v-if="paparWaypointList" key="sap" class="scroll-taktikal" style="background: rgba(15, 23, 42, 0.8); border: 1px solid #334155; border-radius: 8px; padding: 12px; height: 400px; overflow-y: auto;">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; border-bottom: 1px solid #1e293b; padding-bottom: 6px;">
              <span style="font-size: 11px; font-weight: bold; color: #fbbf24;">📋 Search Action Plan (SAP)</span>
              <button @click="paparWaypointList = false" style="background: transparent; border: none; color: #94a3b8; font-size: 14px; cursor: pointer; padding: 0 4px;">✕</button>
            </div>
            
            <div v-if="selectedPlanDetails" style="font-size: 11px; color: #94a3b8; display: flex; flex-direction: column; gap: 5px;">
              <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 4px; background: rgba(30, 41, 59, 0.5); padding: 6px; border-radius: 6px; font-size: 10px;">
                <div>SRU ID: <strong style="color: white;">{{ selectedPlanDetails.sru_name || '---' }}</strong></div>
                <div>Zon: <strong style="color: white;">{{ selectedPlanDetails.zone_name || '---' }}</strong></div>
                <div>Pattern: <strong style="color: white;">{{ selectedPlanDetails.search_pattern }}</strong></div>
                <div>Speed: <strong style="color: white;">{{ selectedPlanDetails.search_speed }} kts</strong></div>
                <div>Length: <strong style="color: white;">{{ selectedPlanDetails.search_area_length }} NM</strong></div>
                <div>Width: <strong style="color: white;">{{ selectedPlanDetails.search_area_width }} NM</strong></div>
                <div>Area: <strong style="color: white;">{{ (selectedPlanDetails.search_area_length * selectedPlanDetails.search_area_width).toFixed(2) }} NM²</strong></div>
                <div>Spacing: <strong style="color: white;">{{ selectedPlanDetails.track_spacing }} NM</strong></div>
              </div>
              
              <div style="margin-top: 6px; font-weight: bold; color: #60a5fa; border-top: 1px solid #1e293b; padding-top: 6px; display: flex; justify-content: space-between; align-items: center;">
                <span>SENARAI WAYPOINT (TEKAN UNTUK PILIH SASARAN):</span>
                <span style="font-size: 10px; color: #10b981; font-family: monospace; font-weight: bold;">{{ progressPercentage }}% SIAP</span>
              </div>

              <!-- CSP Entry jika wujud -->
              <div v-if="activeCspCoord" 
                   @click.stop="currentNavIndex = -1; isSearchCompleted = false; playWaypointChime();"
                   title="Tekan untuk sasarkan CSP"
                   style="margin-bottom: 4px; padding: 6px; border-radius: 4px; display: flex; justify-content: space-between; align-items: center; cursor: pointer; transition: 0.2s;"
                   :style="cspStatus === 'completed' ? 'background: rgba(16, 185, 129, 0.12); border-left: 3px solid #10b981;' : (cspStatus === 'skipped' ? 'background: rgba(245, 158, 11, 0.12); border-left: 3px solid #f59e0b;' : (currentNavIndex === -1 ? 'background: rgba(59, 130, 246, 0.2); border-left: 3px solid #60a5fa; box-shadow: 0 0 8px rgba(96, 165, 250, 0.3);' : 'background: rgba(15, 23, 42, 0.4); border-left: 3px solid #334155;'))">
                <div>
                  <span :style="{ color: cspStatus === 'completed' ? '#10b981' : (cspStatus === 'skipped' ? '#f59e0b' : (currentNavIndex === -1 ? '#60a5fa' : '#94a3b8')), fontWeight: 'bold' }">
                    {{ cspStatus === 'completed' ? '✔ CSP' : (cspStatus === 'skipped' ? '⏭️ CSP' : (currentNavIndex === -1 ? '▶ CSP' : 'CSP')) }}:
                  </span>
                  <span style="font-family: monospace; font-size: 10px; margin-left: 4px; color: #cbd5e1;">{{ formatCoordinate(activeCspCoord[0], true) }} {{ formatCoordinate(activeCspCoord[1], false) }}</span>
                </div>
                <span v-if="cspStatus === 'completed'" style="font-size: 9px; color: #10b981; font-weight: bold; background: rgba(16, 185, 129, 0.2); padding: 1px 5px; border-radius: 3px;">SELESAI</span>
                <span v-else-if="cspStatus === 'skipped'" style="font-size: 9px; color: #f59e0b; font-weight: bold; background: rgba(245, 158, 11, 0.2); padding: 1px 5px; border-radius: 3px;">SKIP</span>
                <span v-else-if="currentNavIndex === -1 && !isSearchCompleted" style="font-size: 9px; background: #3b82f6; color: white; padding: 1px 5px; border-radius: 3px; font-weight: bold;">AKTIF</span>
                <span v-else style="font-size: 9px; color: #64748b;">MENUNGGU</span>
              </div>

              <!-- Waypoint List -->
              <div v-if="activeSortieWaypoints && activeSortieWaypoints.length > 0">
                <div v-for="(wp, index) in activeSortieWaypoints" :key="index" 
                     @click.stop="pilihWpManual(index)"
                     title="Tekan untuk jadikan sasaran aktif"
                     style="margin-bottom: 4px; padding: 6px; border-radius: 4px; cursor: pointer; transition: 0.2s;"
                     :style="getWpItemStyle(index)">
                  <div style="display: flex; justify-content: space-between; align-items: center;">
                    <div>
                      <span :style="{ color: getWpColor(index), fontWeight: 'bold' }">
                        {{ getWpLabel(index) }}:
                      </span>
                      <span style="font-family: monospace; font-size: 10px; margin-left: 4px; color: #cbd5e1;">{{ formatCoordinate(wp[0], true) }} {{ formatCoordinate(wp[1], false) }}</span>
                    </div>
                    <span v-if="isWpCompleted(index)" style="font-size: 9px; color: #10b981; font-weight: bold; background: rgba(16, 185, 129, 0.2); padding: 1px 5px; border-radius: 3px;">SELESAI</span>
                    <span v-else-if="isWpSkipped(index)" style="font-size: 9px; color: #f59e0b; font-weight: bold; background: rgba(245, 158, 11, 0.2); padding: 1px 5px; border-radius: 3px;">SKIP</span>
                    <span v-else-if="index === currentNavIndex && !isSearchCompleted" style="font-size: 9px; background: #3b82f6; color: white; padding: 1px 5px; border-radius: 3px; font-weight: bold;">AKTIF</span>
                    <span v-else style="font-size: 9px; color: #64748b;">MENUNGGU</span>
                  </div>
                  <div v-if="index > 0" style="font-size: 9px; color: #64748b; margin-top: 2px;">
                    (Course: {{ kiraBaringan(activeSortieWaypoints[index-1][0], activeSortieWaypoints[index-1][1], wp[0], wp[1]) }}°, 
                    Length: {{ calculateDistance(activeSortieWaypoints[index-1][0], activeSortieWaypoints[index-1][1], wp[0], wp[1]).toFixed(2) }} NM)
                  </div>
                </div>
              </div>
              <div v-else style="color: #f59e0b;">Tiada waypoint dikesan.</div>
            </div>
            <div v-else style="font-size: 11px; color: #f59e0b;">Sila pilih aset untuk melihat pelan SAR.</div>
          </div>

          <!-- 2. SIGHTING / PENEMUAN OBJEK MODAL -->
          <div v-else-if="paparSightingModal" key="sighting" style="background: rgba(15, 23, 42, 0.95); border: 1px solid #ea580c; border-radius: 8px; padding: 14px; display: flex; flex-direction: column; gap: 10px; max-height: 420px; overflow-y: auto;">
            <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #1e293b; padding-bottom: 6px;">
              <span style="font-size: 12px; font-weight: bold; color: #fb923c;">📍 REKOD PENEMUAN OBJEK (SIGHTING)</span>
              <button @click="paparSightingModal = false" style="background: transparent; border: none; color: #94a3b8; font-size: 14px; cursor: pointer;">✕</button>
            </div>

            <div style="background: rgba(30, 41, 59, 0.6); padding: 8px; border-radius: 6px; font-size: 11px;">
              <div>Koordinat Semasa: <strong style="color: #38bdf8; font-family: monospace;">{{ formatCoordinate(currentLat, true) }} {{ formatCoordinate(currentLng, false) }}</strong></div>
            </div>

            <div style="display: flex; flex-direction: column; gap: 4px;">
              <label style="font-size: 10px; color: #94a3b8; font-weight: bold;">JENIS PENEMUAN:</label>
              <select v-model="sightingType" style="background: #0f172a; border: 1px solid #475569; color: white; padding: 8px; border-radius: 6px; font-size: 11px;">
                <option v-for="j in senaraiJenisSighting" :key="j" :value="j">{{ j }}</option>
              </select>
            </div>

            <div style="display: flex; flex-direction: column; gap: 4px;">
              <label style="font-size: 10px; color: #94a3b8; font-weight: bold;">CATATAN / KETERANGAN OBJEK:</label>
              <input v-model="sightingRemarks" @keyup.enter="rekodSighting" type="text" placeholder="cth: Jaket keselamatan jingga terapung..." style="background: #0f172a; border: 1px solid #475569; color: white; padding: 8px; border-radius: 6px; font-size: 11px;" />
            </div>

            <button @click="rekodSighting" style="background: linear-gradient(135deg, #ea580c, #c2410c); color: white; border: none; padding: 10px; border-radius: 6px; font-weight: bold; font-size: 11px; cursor: pointer; box-shadow: 0 4px 12px rgba(234, 88, 12, 0.4);">
              📍 SAHKAN & HANTAR PENEMUAN KE HQ
            </button>

            <!-- Senarai Rekod Penemuan Sesi Ini -->
            <div v-if="senaraiSightings.length > 0" style="margin-top: 6px; border-top: 1px solid #1e293b; padding-top: 6px;">
              <div style="font-size: 10px; font-weight: bold; color: #94a3b8; margin-bottom: 4px;">REKOD PENEMUAN TERKINI:</div>
              <div v-for="s in senaraiSightings" :key="s.id" style="background: rgba(30, 41, 59, 0.4); padding: 6px; border-radius: 4px; font-size: 10px; margin-bottom: 4px; display: flex; justify-content: space-between;">
                <div>
                  <strong style="color: #fb923c;">{{ s.type }}</strong>
                  <div style="color: #94a3b8;">{{ s.remarks }}</div>
                </div>
                <span style="color: #cbd5e1; font-family: monospace; font-size: 9px;">{{ s.time }}</span>
              </div>
            </div>
          </div>

          <!-- 3. MOB (MAN OVERBOARD) MODAL -->
          <div v-else-if="paparMobModal" key="mob" style="background: rgba(15, 23, 42, 0.95); border: 2px solid #dc2626; border-radius: 8px; padding: 14px; display: flex; flex-direction: column; gap: 10px; text-align: center; box-shadow: 0 10px 30px rgba(220, 38, 38, 0.7);">
            <div style="font-size: 14px; font-weight: bold; color: #f87171; display: flex; align-items: center; justify-content: center; gap: 6px;">
              <span>🚨</span> KECEMASAN MAN OVERBOARD (MOB)
            </div>

            <div v-if="!isMobActive" style="font-size: 11px; color: #cbd5e1;">
              Adakah anda ingin mengaktifkan isyarat kecemasan <strong>MOB</strong> pada kedudukan semasa? Koordinat akan dikunci serta-merta dan amaran kecemasan akan dipancarkan ke HQ.
            </div>

            <div v-else style="background: rgba(220, 38, 38, 0.2); border: 1px solid #ef4444; border-radius: 6px; padding: 10px; text-align: left; font-size: 11px; display: flex; flex-direction: column; gap: 4px;">
              <div style="color: #fca5a5; font-weight: bold;">STATUS: KECEMASAN MOB AKTIF</div>
              <div>Masa Jatuhan: <strong style="color: white;">{{ mobTime }}</strong></div>
              <div>Posisi MOB: <strong style="color: white; font-family: monospace;">{{ formatCoordinate(mobCoord[0], true) }} {{ formatCoordinate(mobCoord[1], false) }}</strong></div>
              <div>Jarak dari Bot: <strong style="color: #4ade80; font-family: monospace;">{{ distanceToMob }} NM</strong></div>
              <div>Arah Haluan ke Titik MOB: <strong style="color: #fbbf24; font-family: monospace;">{{ courseToMob }}°</strong></div>
            </div>

            <div style="display: flex; gap: 8px; justify-content: center; margin-top: 6px;">
              <button v-if="!isMobActive" @click="aktifkanMOB(); paparMobModal = false;" style="flex: 1; background: #dc2626; color: white; border: none; padding: 10px 14px; border-radius: 6px; font-size: 11px; font-weight: bold; cursor: pointer; box-shadow: 0 4px 15px rgba(220, 38, 38, 0.6);">
                🚨 AKTIFKAN MOB SEKARANG
              </button>
              <button v-else @click="batalkanMOB()" style="flex: 1; background: #16a34a; color: white; border: none; padding: 10px 14px; border-radius: 6px; font-size: 11px; font-weight: bold; cursor: pointer;">
                ✔ BATALKAN / SELESAI MOB
              </button>

              <button @click="paparMobModal = false" style="background: #334155; color: #cbd5e1; border: none; padding: 10px 14px; border-radius: 6px; font-size: 11px; font-weight: bold; cursor: pointer;">
                TUTUP
              </button>
            </div>
          </div>

          <!-- 4. CUACA MARITIM & KEADAAN LAUT MODAL (DENGAN 2 TAB LOKASI) -->
          <div v-else-if="paparCuacaModal" key="cuaca" style="background: rgba(15, 23, 42, 0.95); border: 1px solid #38bdf8; border-radius: 8px; padding: 14px; display: flex; flex-direction: column; gap: 10px; max-height: 440px; overflow-y: auto; box-shadow: 0 10px 30px rgba(0,0,0,0.7);">
            <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #1e293b; padding-bottom: 6px;">
              <span style="font-size: 12px; font-weight: bold; color: #38bdf8;">🌦️ DATA CUACA & KEADAAN LAUT</span>
              <button @click="paparCuacaModal = false" style="background: transparent; border: none; color: #94a3b8; font-size: 14px; cursor: pointer; padding: 0 4px;">✕</button>
            </div>

            <!-- 2 Tab Pemilihan Lokasi: Bot vs CSP/Zon Sektor -->
            <div style="display: flex; background: rgba(30, 41, 59, 0.8); padding: 4px; border-radius: 6px; gap: 6px; border: 1px solid #475569;">
              <button @click.stop="tukarTabCuaca('bot')" type="button" style="flex: 1; padding: 8px 6px; border: none; border-radius: 4px; font-size: 11px; font-weight: bold; cursor: pointer; transition: 0.2s; display: flex; align-items: center; justify-content: center; gap: 4px;"
                      :style="tabLokasiCuaca === 'bot' ? 'background: #2563eb; color: white; box-shadow: 0 2px 8px rgba(37,99,235,0.4);' : 'background: transparent; color: #94a3b8;'">
                🚤 LOKASI BOT SEMASA
              </button>
              <button @click.stop="tukarTabCuaca('csp')" type="button" style="flex: 1; padding: 8px 6px; border: none; border-radius: 4px; font-size: 11px; font-weight: bold; cursor: pointer; transition: 0.2s; display: flex; align-items: center; justify-content: center; gap: 4px;"
                      :style="tabLokasiCuaca === 'csp' ? 'background: #2563eb; color: white; box-shadow: 0 2px 8px rgba(37,99,235,0.4);' : 'background: transparent; color: #94a3b8;'">
                🎯 ZON CSP / SEKTOR
              </button>
            </div>

            <div v-if="isMemuatCuaca" style="text-align: center; padding: 25px 10px; color: #94a3b8; font-size: 11px;">
              <div style="font-size: 24px; margin-bottom: 6px; animation: pulse 1s infinite;">🛰️</div>
              <div>Mengambil data ramalan cuaca maritim NOAA/ECMWF...</div>
            </div>

            <div v-else-if="dataCuaca" style="display: flex; flex-direction: column; gap: 8px;">
              <!-- Header Cuaca Semasa -->
              <div style="background: rgba(30, 41, 59, 0.6); padding: 10px; border-radius: 6px; display: flex; justify-content: space-between; align-items: center;">
                <div>
                  <div style="font-size: 10px; color: #94a3b8;">
                    {{ tabLokasiCuaca === 'bot' ? 'Cuaca di Kedudukan Bot:' : 'Cuaca di Titik CSP / Sektor:' }}
                  </div>
                  <div style="font-size: 13px; font-weight: bold; color: #38bdf8; display: flex; align-items: center; gap: 6px; margin-top: 2px;">
                    <span style="font-size: 20px;">{{ tafsirKodCuaca(dataCuaca.weather_code).ikon }}</span>
                    <span>{{ tafsirKodCuaca(dataCuaca.weather_code).teks }}</span>
                  </div>
                </div>
                <div style="text-align: right;">
                  <div style="font-size: 20px; font-weight: bold; color: #f8fafc; font-family: monospace;">{{ dataCuaca.temperature_2m }}°C</div>
                  <div style="font-size: 9px; color: #94a3b8;">Terasa: {{ dataCuaca.apparent_temperature }}°C</div>
                </div>
              </div>

              <!-- Grid Maklumat Taktikal Laut & Angin -->
              <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 6px;">
                <!-- Angin -->
                <div style="background: rgba(30, 41, 59, 0.5); padding: 8px; border-radius: 6px; font-size: 10px; border-left: 3px solid #38bdf8;">
                  <div style="color: #94a3b8; font-weight: bold;">💨 KELAJUAN ANGIN</div>
                  <div style="font-size: 13px; font-weight: bold; color: white; margin-top: 2px; font-family: monospace;">{{ dataCuaca.wind_speed_10m }} kts</div>
                  <div style="color: #cbd5e1; font-size: 9px; margin-top: 1px;">Teras (Gust): {{ dataCuaca.wind_gusts_10m }} kts</div>
                </div>

                <!-- Arah Angin -->
                <div style="background: rgba(30, 41, 59, 0.5); padding: 8px; border-radius: 6px; font-size: 10px; border-left: 3px solid #38bdf8;">
                  <div style="color: #94a3b8; font-weight: bold;">🧭 ARAH ANGIN</div>
                  <div style="font-size: 12px; font-weight: bold; color: white; margin-top: 2px;">{{ tukarArahAngin(dataCuaca.wind_direction_10m) }}</div>
                </div>

                <!-- Ombak / Alun -->
                <div style="background: rgba(30, 41, 59, 0.5); padding: 8px; border-radius: 6px; font-size: 10px; border-left: 3px solid #10b981;">
                  <div style="color: #94a3b8; font-weight: bold;">🌊 KETINGGIAN OMBAK</div>
                  <div style="font-size: 13px; font-weight: bold; color: #4ade80; margin-top: 2px; font-family: monospace;">
                    {{ dataCuaca.wave_height !== null ? dataCuaca.wave_height + ' meter' : 'Pesisir / < 0.3m' }}
                  </div>
                  <div style="color: #cbd5e1; font-size: 9px; margin-top: 1px;">
                    {{ dataCuaca.wave_height !== null ? '(' + (dataCuaca.wave_height * 3.28084).toFixed(1) + ' kaki)' : 'Tenang' }}
                  </div>
                </div>

                <!-- Tempoh Ombak -->
                <div style="background: rgba(30, 41, 59, 0.5); padding: 8px; border-radius: 6px; font-size: 10px; border-left: 3px solid #10b981;">
                  <div style="color: #94a3b8; font-weight: bold;">⏱️ TEMPOH ALUN</div>
                  <div style="font-size: 13px; font-weight: bold; color: #4ade80; margin-top: 2px; font-family: monospace;">
                    {{ dataCuaca.wave_period !== null ? dataCuaca.wave_period + ' saat' : '-' }}
                  </div>
                  <div style="color: #cbd5e1; font-size: 9px; margin-top: 1px;">
                    Arah Alun: {{ dataCuaca.wave_direction !== null ? dataCuaca.wave_direction + '°' : '-' }}
                  </div>
                </div>
              </div>

              <!-- Bar Koordinat & Refresh -->
              <div style="display: flex; justify-content: space-between; align-items: center; font-size: 10px; color: #94a3b8; margin-top: 4px; padding: 6px 8px; background: rgba(15, 23, 42, 0.6); border-radius: 4px;">
                <span>📍 {{ formatCoordinate(lokasiCuacaLat, true) }} {{ formatCoordinate(lokasiCuacaLng, false) }}</span>
                <button @click="muatTurunDataCuaca" style="background: #2563eb; color: white; border: none; padding: 4px 10px; border-radius: 4px; font-size: 10px; font-weight: bold; cursor: pointer;">
                  🔄 MUAT SEMULA
                </button>
              </div>
            </div>

            <div v-else style="color: #f87171; font-size: 11px; text-align: center; padding: 15px;">
              Gagal memuat turun data cuaca maritim. Sila pastikan capaian internet aktif.
            </div>
          </div>

          <!-- 5. MESSAGE PANEL -->
          <div v-else-if="paparChat" key="chat" style="background: rgba(15, 23, 42, 0.85); border: 1px solid #334155; border-radius: 8px; padding: 12px; height: 400px; display: flex; flex-direction: column; gap: 8px;">
            <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #1e293b; padding-bottom: 4px;">
              <span style="font-size: 11px; font-weight: bold; color: #60a5fa;">💬 MESEJ BILIK GERAKAN / HQ</span>
            </div>
            
            <div class="chat-box" style="flex: 1; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; padding-right: 4px;">
              <div v-for="msg in senaraiMesej" :key="msg.id" :style="{
                alignSelf: msg.sender === selectedAsset ? 'flex-end' : 'flex-start',
                background: msg.sender === selectedAsset ? '#2563eb' : '#334155',
                color: 'white', padding: '6px 10px', borderRadius: '6px', maxWidth: '85%', fontSize: '11px'
              }">
                <small style="display: block; font-size: 8px; color: #cbd5e1; margin-bottom: 2px;">{{ msg.sender }}</small>
                {{ msg.message }}
              </div>
            </div>

            <div style="display: flex; gap: 5px; margin-top: 5px;">
              <input v-model="mesejBaharu" @keyup.enter="hantarMesej" type="text" placeholder="Mesej ke HQ..." style="flex: 1; padding: 8px; background: #0f172a; border: 1px solid #475569; color: white; border-radius: 4px; font-size: 11px;" />
              <button @click="hantarMesej" style="background: #2563eb; color: white; border: none; padding: 0 10px; border-radius: 4px; font-weight: bold; font-size: 11px; cursor: pointer;">KIRIM</button>
            </div>
          </div>

          <!-- 5. CONFIRMATION PANEL (DENGAN % KEMAJUAN SIAP) -->
          <div v-else-if="paparConfirmStop" key="confirm" style="background: rgba(15, 23, 42, 0.95); border: 1px solid #dc2626; border-radius: 8px; padding: 14px; display: flex; flex-direction: column; gap: 10px; text-align: center; box-shadow: 0 10px 30px rgba(0,0,0,0.8);">
            <div style="font-size: 13px; font-weight: bold; color: #f87171; display: flex; align-items: center; justify-content: center; gap: 6px;">
              <span>⚠️</span> SAHKAN PENAMATAN MISI
            </div>

            <!-- Ringkasan Kemajuan Misi (% Complete) -->
            <div style="background: rgba(30, 41, 59, 0.7); border: 1px solid #334155; border-radius: 6px; padding: 8px 12px; display: flex; flex-direction: column; gap: 6px; text-align: left;">
              <div style="display: flex; justify-content: space-between; align-items: center; font-size: 11px;">
                <span style="color: #94a3b8;">Status Kemajuan Carian:</span>
                <span :style="{ color: isSearchCompleted ? '#4ade80' : '#fbbf24' }" style="font-weight: bold; font-family: monospace; font-size: 13px;">
                  {{ progressPercentage }}% SIAP
                </span>
              </div>
              
              <!-- Progress Bar Mini -->
              <div style="background: #0f172a; height: 6px; border-radius: 3px; overflow: hidden;">
                <div :style="{ width: progressPercentage + '%' }" style="background: linear-gradient(90deg, #3b82f6, #10b981); height: 100%; transition: width 0.4s ease;"></div>
              </div>

              <div style="display: flex; justify-content: space-between; font-size: 10px; color: #cbd5e1;">
                <span>Selesai: <strong style="color: #4ade80;">{{ completedWpCount }}</strong> | Skip: <strong style="color: #f59e0b;">{{ skippedWpCount }}</strong> / {{ totalTargetsCount }} Titik</span>
                <span :style="{ color: isSearchCompleted && skippedWpCount === 0 ? '#4ade80' : (isSearchCompleted ? '#fbbf24' : '#f87171') }">
                  {{ isSearchCompleted && skippedWpCount === 0 ? '✔ Carian Selesai Penuh' : (isSearchCompleted ? '✔ Carian Tamat (Ada Skip)' : (progressPercentage > 0 ? '⚠️ Penamatan Awal' : 'Belum Bermula')) }}
                </span>
              </div>
            </div>

            <div style="font-size: 11px; color: #cbd5e1;">
              Adakah anda pasti ingin menghentikan penjejakan GPS dan menamatkan misi untuk aset <strong style="color: #60a5fa;">{{ selectedAsset }}</strong>?
            </div>

            <div style="display: flex; gap: 8px; justify-content: center; margin-top: 4px;">
              <button @click="currentScreen = 'setup'; hentiTracking(); paparConfirmStop = false;" style="flex: 1; background: #dc2626; color: white; border: none; padding: 10px 12px; border-radius: 6px; font-size: 11px; font-weight: bold; cursor: pointer; transition: 0.2s;">
                YA, TAMATKAN ({{ progressPercentage }}%)
              </button>
              <button @click="paparConfirmStop = false" style="flex: 1; background: #334155; color: #cbd5e1; border: none; padding: 10px 12px; border-radius: 6px; font-size: 11px; font-weight: bold; cursor: pointer; transition: 0.2s;">
                BATAL
              </button>
            </div>
          </div>
        </transition>

        <!-- Tactical 5-Button Action Bar -->
        <div style="display: flex; gap: 6px; justify-content: space-between; width: 100%;">
          <!-- 1. SAP Modal -->
          <button @click="toggleWaypointList" style="flex: 1; background: rgba(30, 41, 59, 0.7); color: #60a5fa; border: 1px solid #334155; padding: 8px 0; border-radius: 8px; font-size: 8px; font-weight: 800; cursor: pointer; display: flex; flex-direction: column; align-items: center; gap: 1px; transition: 0.2s;">
            <span style="font-size: 15px;">📋</span> SAP
          </button>

          <!-- 2. SIGHTING POI Modal -->
          <button @click="paparSightingModal = !paparSightingModal; paparWaypointList = false; paparMobModal = false; paparConfirmStop = false; paparChat = false" style="flex: 1; background: rgba(30, 41, 59, 0.7); color: #60a5fa; border: 1px solid #334155; padding: 8px 0; border-radius: 8px; font-size: 8px; font-weight: 800; cursor: pointer; display: flex; flex-direction: column; align-items: center; gap: 1px; transition: 0.2s;">
            <span style="font-size: 15px;">📍</span> SIGHTING
          </button>

          <!-- 3. STOP/END (Di Tengah-Tengah) -->
          <button @click="paparConfirmStop = true; paparWaypointList = false; paparSightingModal = false; paparMobModal = false; paparChat = false" style="flex: 1; background: rgba(30, 41, 59, 0.7); color: #f87171; border: 1px solid #334155; padding: 8px 0; border-radius: 8px; font-size: 8px; font-weight: 800; cursor: pointer; display: flex; flex-direction: column; align-items: center; gap: 1px; transition: 0.2s;">
            <span style="font-size: 15px;">🛑</span> STOP
          </button>

          <!-- 4. SKIP WP Quick Button -->
          <button @click="langkauKeWpSeterusnya" style="flex: 1; background: rgba(30, 41, 59, 0.7); color: #60a5fa; border: 1px solid #334155; padding: 8px 0; border-radius: 8px; font-size: 8px; font-weight: 800; cursor: pointer; display: flex; flex-direction: column; align-items: center; gap: 1px; transition: 0.2s;">
            <span style="font-size: 15px;">⏭️</span> SKIP WP
          </button>

          <!-- 5. MESSAGE -->
          <button @click="paparChat = !paparChat; paparWaypointList = false; paparSightingModal = false; paparMobModal = false; paparConfirmStop = false; unreadCount = 0" style="flex: 1; background: rgba(30, 41, 59, 0.7); color: #60a5fa; border: 1px solid #334155; padding: 8px 0; border-radius: 8px; font-size: 8px; font-weight: 800; cursor: pointer; display: flex; flex-direction: column; align-items: center; gap: 1px; transition: 0.2s; position: relative;">
            <span v-if="unreadCount > 0" style="position: absolute; top: -5px; right: 5px; background: #ef4444; color: white; border-radius: 10px; padding: 1px 6px; font-size: 10px; font-weight: bold; border: 2px solid #1e293b;">{{ unreadCount }}</span>
            <span style="font-size: 15px;">💬</span> CHAT
          </button>
        </div>
      </div>

    </div>

  </div>
</template>

<script setup>
import logoApmm from './assets/logo_apmm.png'
import heliBg from './assets/heli.jpg'
import { ref, nextTick, computed, onMounted } from 'vue'
import { createClient } from '@supabase/supabase-js'
import 'leaflet/dist/leaflet.css'
import L from 'leaflet'
import { Geolocation } from '@capacitor/geolocation'

// =========================================================================
// INNESIALISASI DATABASE SUPABASE KAU (CUN MELECUN TERUS)
// =========================================================================
const supabaseUrl = 'https://ilizrmtuurenvlsygllk.supabase.co'
const supabaseKey = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImlsaXpybXR1dXJlbnZsc3lnbGxrIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzkzNDA0NzcsImV4cCI6MjA5NDkxNjQ3N30.kRmx40LqGxp9H2fUPzhbFqHc1LedT1RCXrTWZT-8yXg'
const supabase = createClient(supabaseUrl, supabaseKey)

// PENGURUSAN ID PERANTI & SESI TEMPATAN (SEAMLESS MULTI-DEVICE SESSION)
const getDeviceId = () => {
  let devId = localStorage.getItem('sar_device_id');
  if (!devId) {
    devId = 'dev_' + Math.random().toString(36).substring(2, 9) + '_' + Date.now().toString(36);
    localStorage.setItem('sar_device_id', devId);
  }
  return devId;
};
const deviceId = getDeviceId();

const simpanSesiAktif = (mrsc, caseId, asset) => {
  localStorage.setItem('sar_active_session', JSON.stringify({
    mrsc,
    caseId,
    asset,
    deviceId,
    timestamp: Date.now()
  }));
};

const padamSesiAktif = () => {
  localStorage.removeItem('sar_active_session');
};

const getSesiAktif = () => {
  try {
    const data = localStorage.getItem('sar_active_session');
    return data ? JSON.parse(data) : null;
  } catch (e) {
    return null;
  }
};

// STATE NAVIGATION & LOGIN SMS OTP
const currentScreen = ref('welcome') 
const noTelefon = ref('')
const otpDihantar = ref(false)
const simulasiOtp = ref('')
const inputOtp = ref('')

// STATE SELECTION BOXES
const selectedMrsc = ref('')
const selectedCaseId = ref('')
const selectedAsset = ref('')

// DATA MASTER (DITARIK DARI WEB)
const senaraiKesWebRaw = ref([])
const senaraiKesTapis = ref([])
const senaraiAsetTapis = ref([])

// MAPS, GPS, & TELEMETRY STATES
const map = ref(null)
const userMarker = ref(null)
const currentLat = ref(0)
const currentLng = ref(0)
const watchId = ref(null)
const isTracking = ref(false)
let mapHeartbeatTimer = null
const cspLine = ref(null)
const selectedPlanDetails = ref(null) // State baru untuk menyimpan detail pelan SAROPS
const paparConfirmStop = ref(false) // State untuk popup pengesahan henti misi
const paparWaypointList = ref(false)
const isFollowing = ref(true) 
const trackHistoryCoordinates = ref([]) // Menyimpan array [lat, lng]
let trackPolylineInstance = null // Rujukan untuk garisan Polyline Leaflet
const streetLayer = ref(null)
const satelliteLayer = ref(null)
const labelLayer = ref(null)
const isSatelit = ref(false)
const cspMarker = ref(null)
const wpMarkersList = ref([])

// Koordinat Sasaran CSP & Waypoint Dinamik
const targetCspLat = ref(null);
const targetCspLng = ref(null);
const activeCspCoord = ref(null);
const activeSortieWaypoints = ref([]);
const currentNavIndex = ref(-1); // -1 = Menuju CSP, 0 = WP 1, 1 = WP 2, ...
const isSearchCompleted = ref(false);

// Penjejakan Eksplisit Status Waypoint: SELESAI vs SKIP
const cspStatus = ref('pending'); // 'pending' | 'completed' | 'skipped'
const completedWpIndices = ref([]); // Indeks WP yang benar-benar sampai (<= 0.2 NM)
const skippedWpIndices = ref([]); // Indeks WP yang dilangkau secara manual (Skip WP)

const isWpCompleted = (index) => completedWpIndices.value.includes(index);
const isWpSkipped = (index) => skippedWpIndices.value.includes(index);

const botSpeed = ref(0)
const botHeading = ref('-')
const distanceToTarget = ref('0.00')
const courseToTarget = ref('-')
const distanceToCsp = ref('0.00') // Untuk keserasian data telemetri
const courseToCsp = ref('-')
const telahTibaCsp = ref(false)

// CHAT REALTIME STATES
const paparChat = ref(false)
const senaraiMesej = ref([])
const unreadCount = ref(0)
const mesejBaharu = ref('')

// TACTICAL 7 NEW FEATURES STATES
const isNightMode = ref(false)
const paparSightingModal = ref(false)
const sightingType = ref('Mangsa Terapung')
const sightingRemarks = ref('')
const senaraiSightings = ref([])
const sightingMarkersList = ref([])

const paparMobModal = ref(false)
const isMobActive = ref(false)
const mobCoord = ref(null)
const mobTime = ref('')
let mobMarkerInstance = null
let mobCircleInstance = null

const isOnlineStatus = ref(navigator.onLine)
const offlineQueue = ref([])
const isSyncingOffline = ref(false)

// STATE & FUNGSI CUACA MARITIM & KEADAAN LAUT (OPEN-METEO 2 TABS)
const paparCuacaModal = ref(false);
const tabLokasiCuaca = ref('bot'); // 'bot' | 'csp'
const isMemuatCuaca = ref(false);
const dataCuaca = ref(null);
const lokasiCuacaLat = ref(0);
const lokasiCuacaLng = ref(0);

const tafsirKodCuaca = (code) => {
  const c = Number(code);
  if (isNaN(c) || code === undefined || code === null) return { teks: 'Cerah / Baik', ikon: '☀️' };
  if (c === 0) return { teks: 'Cerah / Baik', ikon: '☀️' };
  if (c === 1 || c === 2) return { teks: 'Sebahagian Berawan', ikon: '🌤️' };
  if (c === 3) return { teks: 'Mendung / Berawan Penuh', ikon: '☁️' };
  if (c === 45 || c === 48) return { teks: 'Berkabus / Kabut Laut', ikon: '🌫️' };
  if (c >= 51 && c <= 55) return { teks: 'Gerimis / Hujan Renyai', ikon: '🌦️' };
  if (c >= 61 && c <= 65) return { teks: 'Hujan', ikon: '🌧️' };
  if (c >= 80 && c <= 82) return { teks: 'Hujan Lebat / Curahan', ikon: '⛈️' };
  if (c >= 95) return { teks: 'Ribut Petir Taktikal', ikon: '🌩️' };
  return { teks: 'Normal / Redup', ikon: '🌤️' };
};

const tukarArahAngin = (deg) => {
  if (deg === null || deg === undefined || isNaN(Number(deg))) return '-';
  const d = Number(deg);
  const kompas = ['U (Utara)', 'TL (Timur Laut)', 'T (Timur)', 'TG (Tenggara)', 'S (Selatan)', 'BD (Barat Daya)', 'B (Barat)', 'BL (Barat Laut)'];
  const idx = Math.round((d % 360) / 45) % 8;
  return `${d}° ${kompas[idx]}`;
};

const muatTurunDataCuaca = async () => {
  let targetLat = 4.21;
  let targetLng = 101.97;

  if (tabLokasiCuaca.value === 'bot') {
    if (currentLat.value !== 0 && currentLng.value !== 0) {
      targetLat = Number(currentLat.value);
      targetLng = Number(currentLng.value);
    } else if (activeCspCoord.value && Array.isArray(activeCspCoord.value)) {
      targetLat = Number(activeCspCoord.value[0]);
      targetLng = Number(activeCspCoord.value[1]);
    }
  } else if (tabLokasiCuaca.value === 'csp') {
    if (activeCspCoord.value && Array.isArray(activeCspCoord.value)) {
      targetLat = Number(activeCspCoord.value[0]);
      targetLng = Number(activeCspCoord.value[1]);
    } else if (activeSortieWaypoints.value && activeSortieWaypoints.value.length > 0) {
      targetLat = Number(activeSortieWaypoints.value[0][0]);
      targetLng = Number(activeSortieWaypoints.value[0][1]);
    } else if (currentLat.value !== 0 && currentLng.value !== 0) {
      targetLat = Number(currentLat.value);
      targetLng = Number(currentLng.value);
    }
  }

  lokasiCuacaLat.value = targetLat;
  lokasiCuacaLng.value = targetLng;
  isMemuatCuaca.value = true;

  try {
    const latFormatted = targetLat.toFixed(4);
    const lngFormatted = targetLng.toFixed(4);
    const weatherUrl = `https://api.open-meteo.com/v1/forecast?latitude=${latFormatted}&longitude=${lngFormatted}&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,weather_code,wind_speed_10m,wind_direction_10m,wind_gusts_10m&wind_speed_unit=kn`;
    const marineUrl = `https://marine-api.open-meteo.com/v1/marine?latitude=${latFormatted}&longitude=${lngFormatted}&current=wave_height,wave_direction,wave_period`;

    const [wRes, mRes] = await Promise.allSettled([
      fetch(weatherUrl).then(r => r.json()),
      fetch(marineUrl).then(r => r.json())
    ]);

    const wData = (wRes.status === 'fulfilled' && wRes.value && wRes.value.current) ? wRes.value.current : null;
    const mData = (mRes.status === 'fulfilled' && mRes.value && mRes.value.current) ? mRes.value.current : null;

    if (wData) {
      dataCuaca.value = {
        temperature_2m: wData.temperature_2m !== undefined ? wData.temperature_2m : '--',
        apparent_temperature: wData.apparent_temperature !== undefined ? wData.apparent_temperature : '--',
        relative_humidity_2m: wData.relative_humidity_2m !== undefined ? wData.relative_humidity_2m : '--',
        precipitation: wData.precipitation !== undefined ? wData.precipitation : 0,
        weather_code: wData.weather_code !== undefined ? wData.weather_code : 0,
        wind_speed_10m: wData.wind_speed_10m !== undefined ? parseFloat(wData.wind_speed_10m.toFixed(1)) : '--',
        wind_direction_10m: wData.wind_direction_10m !== undefined ? wData.wind_direction_10m : 0,
        wind_gusts_10m: wData.wind_gusts_10m !== undefined ? parseFloat(wData.wind_gusts_10m.toFixed(1)) : '--',
        wave_height: mData && mData.wave_height !== undefined && mData.wave_height !== null ? parseFloat(mData.wave_height.toFixed(2)) : null,
        wave_direction: mData && mData.wave_direction !== undefined ? mData.wave_direction : null,
        wave_period: mData && mData.wave_period !== undefined && mData.wave_period !== null ? parseFloat(mData.wave_period.toFixed(1)) : null,
      };
    } else {
      console.warn("Tiada data cuaca dikembalikan dari Open-Meteo");
    }
  } catch (err) {
    console.error("Ralat memuat turun data cuaca:", err);
  } finally {
    isMemuatCuaca.value = false;
  }
};

const tukarTabCuaca = (tab) => {
  tabLokasiCuaca.value = tab;
  muatTurunDataCuaca();
};

const bukaModalCuaca = () => {
  paparCuacaModal.value = !paparCuacaModal.value;
  paparWaypointList.value = false;
  paparSightingModal.value = false;
  paparMobModal.value = false;
  paparChat.value = false;
  paparConfirmStop.value = false;

  if (paparCuacaModal.value) {
    muatTurunDataCuaca();
  }
};

try {
  const savedQueue = localStorage.getItem('sar_offline_tracks')
  if (savedQueue) offlineQueue.value = JSON.parse(savedQueue)
} catch (e) {}

const senaraiJenisSighting = [
  'Mangsa Terapung',
  'Jaket Keselamatan',
  'Serpihan / Debris',
  'Bot Terbalik / Karam',
  'Tumpahan Minyak',
  'Isyarat Flare / Asap',
  'Lain-lain Objek'
]

// =========================================================================
// ENJIN AUDIO & HAPTIC TACTICAL ALERTS (WEB AUDIO SYNTHESIZER)
// =========================================================================
const playAudioTone = (freq = 880, type = 'sine', duration = 0.15) => {
  try {
    const AudioCtx = window.AudioContext || window.webkitAudioContext
    if (!AudioCtx) return
    const ctx = new AudioCtx()
    const osc = ctx.createOscillator()
    const gain = ctx.createGain()
    osc.type = type
    osc.frequency.setValueAtTime(freq, ctx.currentTime)
    gain.gain.setValueAtTime(0.2, ctx.currentTime)
    gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + duration)
    osc.connect(gain)
    gain.connect(ctx.destination)
    osc.start()
    osc.stop(ctx.currentTime + duration)
  } catch (e) {
    console.warn("Audio Synthesizer:", e)
  }
}

const playWaypointChime = () => {
  playAudioTone(880, 'sine', 0.12)
  setTimeout(() => playAudioTone(1320, 'sine', 0.25), 130)
  if (navigator.vibrate) {
    navigator.vibrate([150, 80, 150])
  }
}

const playIncomingMessageSound = () => {
  playAudioTone(880, 'sine', 0.1)
  setTimeout(() => playAudioTone(1174, 'sine', 0.18), 100)
  if (navigator.vibrate) {
    navigator.vibrate([120, 60, 120])
  }
}

const playSightingSound = () => {
  playAudioTone(600, 'triangle', 0.12)
  setTimeout(() => playAudioTone(900, 'triangle', 0.2), 120)
  if (navigator.vibrate) {
    navigator.vibrate([200, 100, 200])
  }
}

const playMobAlarm = () => {
  for (let i = 0; i < 6; i++) {
    setTimeout(() => playAudioTone(950, 'sawtooth', 0.15), i * 180)
  }
  if (navigator.vibrate) {
    navigator.vibrate([500, 150, 500, 150, 500, 150, 500])
  }
}

// STATE NOTIFIKASI TOAST TERAPUNG (IN-APP TOAST BANNER)
const toastNotifikasi = ref({
  papar: false,
  jenis: 'mesej', // 'mesej' | 'mob' | 'sighting'
  ikon: '💬',
  tajuk: '',
  mesej: '',
  timer: null
})

const tunjukToastNotifikasi = (jenis, ikon, tajuk, mesej, tempoh = 6000) => {
  if (toastNotifikasi.value.timer) {
    clearTimeout(toastNotifikasi.value.timer)
  }
  toastNotifikasi.value = {
    papar: true,
    jenis,
    ikon,
    tajuk,
    mesej,
    timer: setTimeout(() => {
      toastNotifikasi.value.papar = false
    }, tempoh)
  }
}

const klikToastNotifikasi = () => {
  if (toastNotifikasi.value.jenis === 'mob') {
    paparMobModal.value = true
  } else if (toastNotifikasi.value.jenis === 'sighting') {
    paparSightingModal.value = true
  } else {
    paparChat.value = true
    unreadCount.value = 0
  }
  toastNotifikasi.value.papar = false
}

const mintaIzinNotifikasi = () => {
  try {
    if ('Notification' in window && Notification.permission === 'default') {
      Notification.requestPermission()
    }
  } catch (e) {}
}

const hantarNotifikasiOS = (title, body) => {
  try {
    if ('Notification' in window && Notification.permission === 'granted') {
      new Notification(title, {
        body: body,
        icon: logoApmm,
        silent: false
      })
    }
  } catch (e) {}
}

const distanceToMob = computed(() => {
  if (!mobCoord.value || currentLat.value === 0) return '0.00'
  return calculateDistance(currentLat.value, currentLng.value, mobCoord.value[0], mobCoord.value[1]).toFixed(2)
})

const courseToMob = computed(() => {
  if (!mobCoord.value || currentLat.value === 0) return '-'
  return kiraBaringan(currentLat.value, currentLng.value, mobCoord.value[0], mobCoord.value[1])
})

// Label Sasaran Semasa
const activeTargetLabel = computed(() => {
  if (isSearchCompleted.value) return 'SELESAI';
  if (currentNavIndex.value === -1) return 'CSP';
  if (currentNavIndex.value >= 0 && currentNavIndex.value < activeSortieWaypoints.value.length) {
    return `WP ${currentNavIndex.value + 1}/${activeSortieWaypoints.value.length}`;
  }
  return '-';
});

// Kiraan Jumlah Sasaran & Kemajuan (%)
const totalTargetsCount = computed(() => {
  let count = activeSortieWaypoints.value.length;
  if (activeCspCoord.value) count += 1;
  return count;
});

const completedWpCount = computed(() => {
  let done = completedWpIndices.value.length;
  if (activeCspCoord.value && cspStatus.value === 'completed') {
    done += 1;
  }
  return done;
});

const skippedWpCount = computed(() => {
  let skipped = skippedWpIndices.value.length;
  if (activeCspCoord.value && cspStatus.value === 'skipped') {
    skipped += 1;
  }
  return skipped;
});

const progressPercentage = computed(() => {
  if (totalTargetsCount.value === 0) return 0;
  if (isSearchCompleted.value && skippedWpCount.value === 0) return 100;
  // Peratusan titik yang BENAR-BENAR SELESAI dilalui (<= 0.2 NM)
  const pct = Math.round((completedWpCount.value / totalTargetsCount.value) * 100);
  return Math.min(100, Math.max(0, pct));
});

// Helper visual untuk senarai waypoint dalam modal SAP
const getWpItemStyle = (index) => {
  if (isWpCompleted(index)) {
    return 'background: rgba(16, 185, 129, 0.12); border-left: 3px solid #10b981;';
  } else if (isWpSkipped(index)) {
    return 'background: rgba(245, 158, 11, 0.12); border-left: 3px solid #f59e0b;';
  } else if (index === currentNavIndex.value && !isSearchCompleted.value) {
    return 'background: rgba(59, 130, 246, 0.2); border-left: 3px solid #60a5fa; box-shadow: 0 0 8px rgba(96, 165, 250, 0.3);';
  } else {
    return 'background: rgba(15, 23, 42, 0.4); border-left: 3px solid #334155;';
  }
};

const getWpColor = (index) => {
  if (isWpCompleted(index)) return '#10b981';
  if (isWpSkipped(index)) return '#f59e0b';
  if (index === currentNavIndex.value && !isSearchCompleted.value) return '#60a5fa';
  return '#94a3b8';
};

const getWpLabel = (index) => {
  if (isWpCompleted(index)) return `✔ WP ${index + 1}`;
  if (isWpSkipped(index)) return `⏭️ WP ${index + 1}`;
  if (index === currentNavIndex.value && !isSearchCompleted.value) return `▶ WP ${index + 1}`;
  return `WP ${index + 1}`;
};

// LUKISAN LOGIK ETA KOMPUTASI
const computedETA = computed(() => {
  const speed = parseFloat(botSpeed.value);
  const distance = parseFloat(distanceToTarget.value);

  if (isNaN(speed) || speed <= 0.5 || isNaN(distance) || distance <= 0) {
    return '-- Min';
  }

  // Formula: Masa (Jam) = Jarak (NM) / Kelajuan (Knot)
  const totalHours = distance / speed;
  const totalMinutes = Math.round(totalHours * 60);

  if (totalMinutes < 60) {
    return `${totalMinutes} Minit`;
  } else {
    const hours = Math.floor(totalMinutes / 60);
    const mins = totalMinutes % 60;
    return `${hours} Jam ${mins} Min`;
  }
});

// PEMETAAN JURISDIKSI REGION APMM (PADANAN FAIL WEB KAU)
const pemetaanMrscKeRegion = {
  'MRSC Langkawi': 'UTARA',
  'MRSC Klang': 'BARAT',
  'MRSC Johor Baharu': 'SELATAN',
  'MRSC Kuantan': 'TIMUR',
  'MRSC Kota Kinabalu': 'SABAH',
  'MRSC Kuching': 'SARAWAK'
}

// =========================================================================
// LOGIK FASA A: SIMULASI SMS OTP CODES
// =========================================================================
const hantarSmsOtp = () => {
  simulasiOtp.value = '1111'
  otpDihantar.value = true
}

const sahkanSmsOtp = () => {
  if (inputOtp.value === '1111') {
    currentScreen.value = 'setup'
    tarikDataAsalKesDariWeb()
    return
  }

  if (inputOtp.value === simulasiOtp.value) {
    currentScreen.value = 'setup'
    tarikDataAsalKesDariWeb()
  } else {
    alert("❌ Kod OTP tidak sah! Sila semak semula kod simulasi.")
  }
}

// =========================================================================
// LOGIK FASA B: PENAPISAN DAN KAWALAN INTEGRASI DATABASE WEB
// =========================================================================
const tarikDataAsalKesDariWeb = async () => {
  const { data, error } = await supabase.from('sar_incidents').select('*')
  if (!error && data) {
    senaraiKesWebRaw.value = data
  }
}

onMounted(async () => {
  await tarikDataAsalKesDariWeb();

  // Semak jika peranti ini mempunyai sesi aktif yang belum ditamatkan (Auto-Resume)
  const activeSession = getSesiAktif();
  if (activeSession && activeSession.mrsc && activeSession.caseId && activeSession.asset) {
    selectedMrsc.value = activeSession.mrsc;
    const kawasanTaktikal = pemetaanMrscKeRegion[selectedMrsc.value];
    if (kawasanTaktikal && senaraiKesWebRaw.value) {
      senaraiKesTapis.value = senaraiKesWebRaw.value.filter(
        kes => kes.status === 'active' && kes.region === kawasanTaktikal
      );
    }
    selectedCaseId.value = activeSession.caseId;
    selectedAsset.value = activeSession.asset;
    
    console.log("🔄 Menyambung semula sesi aktif peranti ini secara automatik:", activeSession);
    await mulaSync();
  }
});

const kendalikanTukarMrsc = () => {
  selectedCaseId.value = ''
  selectedAsset.value = ''
  senaraiAsetTapis.value = []
  
  const kawasanTaktikal = pemetaanMrscKeRegion[selectedMrsc.value]
  senaraiKesTapis.value = senaraiKesWebRaw.value.filter(
    kes => kes.status === 'active' && kes.region === kawasanTaktikal
  )
}

let setupPresenceTimer = null;
let telemetryPresenceChannel = null;

const muatSemulaStatusAset = async () => {
  if (!selectedCaseId.value) return;

  try {
    const { data: dataPelan, error: errorPelan } = await supabase
      .from('sar_plans')
      .select('id, sru_name')
      .eq('case_id', Number(selectedCaseId.value));

    const { data: dataTelemetry, error: errorTele } = await supabase
      .from('sru_telemetry')
      .select('*');

    if (!errorPelan && dataPelan) {
      const activeSession = getSesiAktif();
      const cutoffTime = new Date(Date.now() - 15 * 60 * 1000);
      const sruAktif = (dataTelemetry || [])
        .filter(t => new Date(t.created_at) > cutoffTime)
        .map(t => (t.boat_id || t.sru_name || '').trim().toLowerCase())
        .filter(Boolean);

      senaraiAsetTapis.value = dataPelan.map(aset => {
        const cleanName = (aset.sru_name || '').trim().toLowerCase();
        const isTakenByOthers = sruAktif.includes(cleanName);
        const isMySession = !!(activeSession && 
          activeSession.asset && 
          activeSession.asset.trim().toLowerCase() === cleanName &&
          Number(activeSession.caseId) === Number(selectedCaseId.value));

        return {
          id: aset.id,
          sru_name: aset.sru_name,
          isTaken: isTakenByOthers && !isMySession, // Hanya kunci jika diambil peranti LAIN
          isMySession: isMySession
        };
      });

      // Sekiranya aset yang sedang dipilih tiba-tiba diambil oleh peranti lain, batalkan pilihan
      if (selectedAsset.value) {
        const asetDipilih = senaraiAsetTapis.value.find(a => a.sru_name === selectedAsset.value);
        if (asetDipilih && asetDipilih.isTaken && !asetDipilih.isMySession) {
          selectedAsset.value = '';
        }
      }
    }
  } catch (err) {
    console.error("Ralat muat semula status aset:", err);
  }
};

const langganPresenceAset = () => {
  if (telemetryPresenceChannel) {
    supabase.removeChannel(telemetryPresenceChannel);
  }
  telemetryPresenceChannel = supabase
    .channel('sru_telemetry_presence_live')
    .on('postgres_changes', { event: '*', schema: 'public', table: 'sru_telemetry' }, () => {
      if (currentScreen.value === 'setup' && selectedCaseId.value) {
        muatSemulaStatusAset();
      }
    })
    .subscribe();

  if (setupPresenceTimer) clearInterval(setupPresenceTimer);
  setupPresenceTimer = setInterval(() => {
    if (currentScreen.value === 'setup' && selectedCaseId.value) {
      muatSemulaStatusAset();
    }
  }, 2000);
};

const kendalikanTukarKes = async () => {
  selectedAsset.value = ''
  telahTibaCsp.value = false
  currentNavIndex.value = -1
  isSearchCompleted.value = false
  activeSortieWaypoints.value = []
  activeCspCoord.value = null
  targetCspLat.value = null; // Reset CSP target when case changes
  targetCspLng.value = null; // Reset CSP target when case changes
  distanceToTarget.value = '0.00'
  courseToTarget.value = '-'
  distanceToCsp.value = '0.00'
  courseToCsp.value = '-'
  if (!selectedCaseId.value) return

  await muatSemulaStatusAset();
  langganPresenceAset();
}

// =========================================================================
// LOGIK FASA C: PENJEJAKAN GPS MAPS & REALTIME BROADCAST TELEMETRY
// =========================================================================
const mulaSync = async () => {
  if (!selectedAsset.value || !selectedCaseId.value) return;

  const activeSession = getSesiAktif();
  const isMyOwnSession = activeSession && 
    activeSession.asset && 
    activeSession.asset.trim().toLowerCase() === selectedAsset.value.trim().toLowerCase();

  // 1. Semakan terakhir sebelum mula untuk elakkan perlumbaan dua peranti (jika bukan sesi sendiri)
  if (!isMyOwnSession) {
    const { data: currentTele } = await supabase
      .from('sru_telemetry')
      .select('*');

    const cutoffTime = new Date(Date.now() - 15 * 60 * 1000);
    const sruAktif = (currentTele || [])
      .filter(t => new Date(t.created_at) > cutoffTime)
      .map(t => (t.boat_id || t.sru_name || '').trim().toLowerCase());

    if (sruAktif.includes(selectedAsset.value.trim().toLowerCase())) {
      alert(`⚠️ Aset ${selectedAsset.value} sedang aktif digunakan oleh peranti lain! Sila pilih aset lain.`);
      await muatSemulaStatusAset();
      return;
    }
  }

  // 2. Simpan sesi aktif dalam localStorage peranti ini
  simpanSesiAktif(selectedMrsc.value, selectedCaseId.value, selectedAsset.value);

  // 3. Kunci aset serta-merta dalam jadual sru_telemetry (HANYA KOLUM YANG WUJUD DALAM SCHEMA)
  try {
    await supabase.from('sru_telemetry').delete().eq('boat_id', selectedAsset.value);
    const { error: insErr } = await supabase.from('sru_telemetry').insert([
      {
        boat_id: selectedAsset.value,
        latitude: currentLat.value || 0,
        longitude: currentLng.value || 0,
        speed: 0,
        course: 0,
        csp: 0
      }
    ]);
    if (insErr) {
      console.error("Ralat pendaftaran kunci aset:", insErr.message);
    }
  } catch (err) {
    console.error("Gagal mendaftarkan kunci aset:", err);
  }

  if (setupPresenceTimer) {
    clearInterval(setupPresenceTimer);
    setupPresenceTimer = null;
  }

  currentScreen.value = 'map'
  await nextTick()
  initMap()
  mulaTracking()
  muatTurunDetailPelanSAROPS()
  muatTurunMesej()
  dengarChatLive() // Menghidupkan talian realtime secara tersusun
}

const initMap = () => {
  const mapContainer = document.getElementById('mapContainer') || document.getElementById('map');
  if (!mapContainer || map.value) return;

  // 1. Cipta Enjin Peta Leaflet
  const enjinPeta = L.map(mapContainer, { zoomControl: false }).setView([4.2105, 101.9758], 6);

  // 2. Sediakan Lapisan-Lapisan (Simpan dalam refs supaya boleh diakses oleh tukarModPeta)
  streetLayer.value = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors',
    zIndex: 1
  });

  satelliteLayer.value = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
    attribution: 'Tiles © Esri',
    zIndex: 1
  });

  labelLayer.value = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/Reference/World_Boundaries_and_Places/MapServer/tile/{z}/{y}/{x}', {
    attribution: 'Labels © Esri',
    zIndex: 2
  });

  const seaMapLayer = L.tileLayer('https://tiles.openseamap.org/seamark/{z}/{x}/{y}.png', {
    attribution: '© OpenSeaMap contributors',
    zIndex: 10
  });

  // 3. Masukkan lapisan awal (Peta Biasa + Seamarks) sebagai DEFAULT
  streetLayer.value.addTo(enjinPeta);
  seaMapLayer.addTo(enjinPeta);
  isSatelit.value = false;

  map.value = enjinPeta;

  // INTEGRASI: Kekalkan listener dragstart supaya fungsi follow/recenter tidak ralat
  map.value.on('dragstart', () => {
    isFollowing.value = false
  });

  muatTurunPelanSAROPSDariWeb();
};

const tukarModPeta = () => {
  if (!map.value || !streetLayer.value || !satelliteLayer.value) return;
  
  if (isSatelit.value) {
    // Balik ke mod Peta Biasa
    map.value.removeLayer(satelliteLayer.value);
    map.value.removeLayer(labelLayer.value);
    streetLayer.value.addTo(map.value);
    isSatelit.value = false;
  } else {
    // Tukar ke mod Satelit
    map.value.removeLayer(streetLayer.value);
    satelliteLayer.value.addTo(map.value);
    labelLayer.value.addTo(map.value);
    isSatelit.value = true;
  }
};

const muatTurunDetailPelanSAROPS = async () => {
  if (!selectedCaseId.value || !selectedAsset.value) {
    return;
  }
  try {
    const { data, error } = await supabase
      .from('sar_plans')
      .select('*')
      .eq('case_id', Number(selectedCaseId.value));

    if (!error && data && data.length > 0) {
      const cleanAsset = selectedAsset.value.trim().toLowerCase();
      const pelan = data.find(p => p.sru_name && p.sru_name.trim().toLowerCase() === cleanAsset) || data[0];

      if (pelan) {
        selectedPlanDetails.value = {
          ...pelan,
          search_pattern: pelan.search_pattern || pelan.pattern_type || pelan.pattern_name || 'PARALLEL SEARCH',
          search_speed: pelan.search_speed || pelan.speed || 0,
          search_area_length: parseFloat(pelan.search_area_length || pelan.length || pelan.search_length || 0),
          search_area_width: parseFloat(pelan.search_area_width || pelan.width || pelan.search_width || 0),
          track_spacing: pelan.track_spacing || pelan.spacing || 0
        };

        if (pelan.sortie_waypoints && Array.isArray(pelan.sortie_waypoints) && pelan.sortie_waypoints.length > 0) {
          activeSortieWaypoints.value = pelan.sortie_waypoints.filter(p => p !== null && Array.isArray(p) && p.length >= 2);
        }
        if (pelan.csp_coord && Array.isArray(pelan.csp_coord) && pelan.csp_coord.length >= 2) {
          activeCspCoord.value = pelan.csp_coord;
          targetCspLat.value = pelan.csp_coord[0];
          targetCspLng.value = pelan.csp_coord[1];
        }
        console.log("📡 DATA INDUK PELAN BERJAYA DISERAGAMKAN:", selectedPlanDetails.value);
      }
    }
  } catch (err) {
    console.error("Ralat muat turun detail pelan SAROPS:", err);
  }
};

const muatTurunPelanSAROPSDariWeb = async () => {
  if (!selectedCaseId.value) return;

  // Bersihkan penanda waypoint sedia ada pada peta
  wpMarkersList.value.forEach(m => {
    if (map.value && m) map.value.removeLayer(m);
  });
  wpMarkersList.value = [];

  const { data, error } = await supabase
    .from('sar_plans')
    .select('*')
    .eq('case_id', Number(selectedCaseId.value));

  if (error) {
    console.error("Gagal memuat turun pelan SAROPS:", error.message);
    return;
  }

  if (data) {
    data.forEach(pelan => {
      const warnaTema = '#10b981'; // Hijau taktikal standard SRU
      const cleanAsset = selectedAsset.value ? selectedAsset.value.trim().toLowerCase() : '';
      const isMyAsset = pelan.sru_name && cleanAsset && (pelan.sru_name.trim().toLowerCase() === cleanAsset);

      // 1. Lukis Kotak Sempadan Kawasan Carian (Search Area Polygon)
      if (pelan.corner_points && Array.isArray(pelan.corner_points) && pelan.corner_points.length >= 4) {
        const pts = pelan.corner_points.filter(p => p !== null && Array.isArray(p) && p.length >= 2);
        if (pts.length >= 4) {
          // Guna L.polygon untuk melukis kawasan tertutup bersegi
          L.polygon(pts, { 
            color: warnaTema, 
            weight: 2, 
            fillColor: warnaTema, 
            fillOpacity: 0.1 
          }).addTo(map.value).bindTooltip(`ZON KAWASAN: ${pelan.zone_name || 'ZON'}`);
        }
      }

      // 2. Lukis Garisan Laluan Carian Bot (Sortie Track / Waypoints)
      if (pelan.sortie_waypoints && Array.isArray(pelan.sortie_waypoints) && pelan.sortie_waypoints.length > 0) {
        const waypoints = pelan.sortie_waypoints.filter(p => p !== null && Array.isArray(p) && p.length >= 2);
        if (waypoints.length > 0) {
          // Guna L.polyline dengan dashArray untuk garisan taktikal putus-putus
          L.polyline(waypoints, { 
            color: '#fbbf24', // Warna oren/kuning amaran untuk track laluan
            weight: 2, 
            dashArray: '5, 8', 
            opacity: 0.9 
          }).addTo(map.value);

          // Jika ini milik aset yang sedang dipilih pengguna, simpan waypoints & lukis penanda bernombor
          if (isMyAsset) {
            activeSortieWaypoints.value = waypoints;
            selectedPlanDetails.value = {
              ...pelan,
              search_pattern: pelan.search_pattern || pelan.pattern_type || pelan.pattern_name || 'PARALLEL SEARCH',
              search_speed: pelan.search_speed || pelan.speed || 0,
              search_area_length: parseFloat(pelan.search_area_length || pelan.length || pelan.search_length || 0),
              search_area_width: parseFloat(pelan.search_area_width || pelan.width || pelan.search_width || 0),
              track_spacing: pelan.track_spacing || pelan.spacing || 0
            };
            waypoints.forEach((wp, idx) => {
              const wpMarker = L.circleMarker(wp, {
                color: '#38bdf8',
                fillColor: '#0284c7',
                fillOpacity: 0.9,
                radius: 4
              }).addTo(map.value).bindTooltip(`WP ${idx + 1}`, { permanent: false, direction: 'top' });
              wpMarkersList.value.push(wpMarker);
            });
          }
        }
      }

      // 3. Lukis Titik Mula Carian (CSP - Commence Search Point)
      if (pelan.csp_coord && Array.isArray(pelan.csp_coord) && pelan.csp_coord.length >= 2) {
        const cspM = L.circleMarker(pelan.csp_coord, { 
          color: '#ef4444', 
          fillColor: '#ef4444', 
          fillOpacity: 1, 
          radius: 5 
        }).addTo(map.value).bindTooltip(`CSP (${pelan.sru_name})`, { permanent: false, direction: 'top' });
        wpMarkersList.value.push(cspM);

        // Jika CSP ini milik aset yang sedang dipilih, set sebagai target untuk pengiraan jarak dan garisan
        if (isMyAsset) {
          activeCspCoord.value = pelan.csp_coord;
          targetCspLat.value = pelan.csp_coord[0];
          targetCspLng.value = pelan.csp_coord[1];
          if (currentNavIndex.value === -1 && !telahTibaCsp.value) {
            currentNavIndex.value = -1; // Mula dari CSP
          }
          console.log(`📡 CSP Dinamik Diterima [${selectedAsset.value}]: ${targetCspLat.value}, ${targetCspLng.value}`);
        }
      }
    });

    // Jika tiada CSP tetapi ada waypoint, mulakan navigasi terus dari WP 1
    if (!activeCspCoord.value && activeSortieWaypoints.value.length > 0 && currentNavIndex.value === -1) {
      currentNavIndex.value = 0;
    }

    // Fokuskan peta secara automatik ke kawasan carian kes yang dipilih
    fokusKeKawasanSAR();
  }
};

// Cipta fungsi pembantu ini di bawah muatTurunPelanSAROPSDariWeb untuk auto-focus peta
const fokusKeKawasanSAR = async () => {
  const { data } = await supabase
    .from('sar_plans')
    .select('corner_points')
    .eq('case_id', Number(selectedCaseId.value));

  if (data && data.length > 0 && map.value) {
    const boundsKoleksi = [];
    data.forEach(p => {
      if (p.corner_points && Array.isArray(p.corner_points) && p.corner_points.length > 0) {
        p.corner_points.forEach(pt => {
          if (pt) boundsKoleksi.push(pt);
        });
      }
    });
    if (boundsKoleksi.length > 0) {
      map.value.fitBounds(L.latLngBounds(boundsKoleksi), { padding: [40, 40] });
    }
  }
};

const mulaTracking = async () => {
  try {
    isTracking.value = true;
    console.log("Memulakan penjejakan GPS...");

    try {
      // Mod Peranti Mudah Alih (Capacitor Engine)
      const permission = await Geolocation.requestPermissions()
      if (permission.location !== 'granted') {
        alert('Kebenaran Lokasi diperlukan untuk operasi SAR!');
        isTracking.value = false; // Set tracking to false if permission denied
        console.warn("Kebenaran lokasi ditolak.");
        return;
      }
      console.log("Kebenaran lokasi diberikan.");

      watchId.value = await Geolocation.watchPosition({
        enableHighAccuracy: true,
        timeout: 10000,
        maximumAge: 0 // Force fresh position
      }, (position, err) => { // Capacitor's watchPosition callback has two arguments
        if (err) {
          console.error("Ralat Capacitor Geolocation watchPosition:", err);
          // Optionally, alert the user or stop tracking if errors persist
          // alert('Ralat GPS peranti: ' + err.message);
          // hentiTracking(); // Consider stopping if critical error
          return;
        }
        if (position) {
          console.log("Posisi GPS diterima (Capacitor):", position.coords.latitude, position.coords.longitude, "Speed:", position.coords.speed, "Heading:", position.coords.heading);
          kemaskiniLokasiSatelit(position.coords.latitude, position.coords.longitude, position.coords.speed, position.coords.heading);
        } else {
          console.warn("Tiada posisi GPS diterima dari Capacitor.");
        }
      })
    } catch (webError) {
      // Mod Browser/Laptop Fallback (Penyelamat)
      if (webError.message.includes('Not implemented on web') || !window.Capacitor) { // Check if Capacitor is not available
        console.log("Menggunakan GPS Web Browser...");
        watchId.value = navigator.geolocation.watchPosition((position) => {
          console.log("Posisi GPS diterima (Web):", position.coords.latitude, position.coords.longitude, "Speed:", position.coords.speed, "Heading:", position.coords.heading);
          kemaskiniLokasiSatelit(position.coords.latitude, position.coords.longitude, position.coords.speed, position.coords.heading);
        }, (err) => {
          console.error("Ralat Web Geolocation watchPosition:", err);
          alert('Ralat GPS pelayar: ' + err.message);
          isTracking.value = false; // Set tracking to false on web error
        }, { enableHighAccuracy: true, timeout: 10000, maximumAge: 0 });
      } else {
        // This catch block might also be hit for other Capacitor errors not related to 'Not implemented on web'
        console.error("Ralat umum Capacitor Geolocation:", webError);
        alert('Ralat umum GPS peranti: ' + webError.message);
        isTracking.value = false;
      }
    }
  } catch (e) {
    alert('Ralat membaca GPS hardware: ' + e.message);
    isTracking.value = false;
  }
}

const formatCoordinate = (coord, isLat) => {
  if (coord === 0 || coord === null || coord === undefined) {
    return isLat ? "00 00.000N" : "000 00.000E";
  }
  
  const absolute = Math.abs(coord);
  const degrees = Math.floor(absolute);
  const minutes = ((absolute - degrees) * 60).toFixed(3);
  
  const paddedDegrees = isLat 
    ? degrees.toString().padStart(2, '0') 
    : degrees.toString().padStart(3, '0');
  const paddedMinutes = minutes.padStart(6, '0'); 
  const suffix = isLat ? (coord >= 0 ? 'N' : 'S') : (coord >= 0 ? 'E' : 'W');
  
  return `${paddedDegrees} ${paddedMinutes}${suffix}`;
};

const kemaskiniLokasiSatelit = (lat, lng, speed, heading) => {
  currentLat.value = lat;
  currentLng.value = lng;
  
  // LOGIK TRACK HISTORY
  if (lat && lng && lat !== 0 && lng !== 0) {
    trackHistoryCoordinates.value.push([lat, lng]);
    
    // Lukis atau kemaskini garisan sejarah atas peta peranti
    if (map.value) {
      if (!trackPolylineInstance) {
        trackPolylineInstance = L.polyline(trackHistoryCoordinates.value, {
          color: '#fbbf24', // Warna oren/kuning emas taktikal
          weight: 3,
          dashArray: '5, 5', // Garisan putus-putus sejarah
          opacity: 0.8
        }).addTo(map.value);
      } else {
        trackPolylineInstance.setLatLngs(trackHistoryCoordinates.value);
      }
    }

    // Hantar rekod jejak GPS ini ke Supabase
    hantarTrackHistoryKeWeb(lat, lng);
  }

  // 1. Baca kelajuan live (jika peranti pegun atau speed null, letak 0)
  // Geolocation bagi m/s, kita tukar ke Knots (kts) dengan darab 1.94384
  if (speed !== null && speed !== undefined) {
    botSpeed.value = speed * 1.94384;
  } else {
    botSpeed.value = 0;
  }

  // 2. Baca arah pergerakan live (heading/course).
  // Jika kelajuan bot 0, paparkan '-' untuk heading. Jika tidak, guna heading sebenar.
  if (botSpeed.value > 0.5) { // Anggap bot bergerak jika kelajuan lebih dari 0.5 kts
    if (heading !== null && heading !== undefined) {
      botHeading.value = heading;
    }
  } else {
    botHeading.value = '-'; // Bot tidak bergerak, paparkan '-'
  }


  // Ikon Marker Dinamik (Arrow) untuk pusingan heading
  const userIcon = L.divIcon({
    className: 'user-marker-icon',
    html: `<div style="transform: rotate(${botHeading.value !== '-' ? botHeading.value : 0}deg); transition: transform 0.3s ease-out;">
             <svg width="32" height="32" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
               <path d="M12 2L4.5 20.29L5.21 21L12 18L18.79 21L19.5 20.29L12 2Z" fill="#00ffff" stroke="#013333" stroke-width="1"/>
             </svg>
           </div>`,
    iconSize: [32, 32],
    iconAnchor: [16, 16]
  });

  // 3. Update marker bot cyan atas peta peranti mudah alih
  if (!userMarker.value) {
    userMarker.value = L.marker([lat, lng], { icon: userIcon }).addTo(map.value);
    map.value.setView([lat, lng], 15);
  } else {
    userMarker.value.setLatLng([lat, lng]);
    userMarker.value.setIcon(userIcon);
  }

  // Logik auto-follow dengan anjakan offset Y
  if (isFollowing.value) {
    const zoom = map.value.getZoom();
    const userP = map.value.project([lat, lng], zoom);
    const shiftedP = userP.subtract([0, 100]); // Anjakkan titik tengah peta ke atas supaya marker di bawah
    const shiftedL = map.value.unproject(shiftedP, zoom);
    map.value.panTo(shiftedL);
  }

  // 4. Kira Jarak & Baringan Navigasi Sasaran Dinamik (CSP -> WP 1 -> WP 2 -> ... -> Selesai)
  let targetCoord = null;
  if (!isSearchCompleted.value) {
    if (currentNavIndex.value === -1 && activeCspCoord.value) {
      targetCoord = activeCspCoord.value;
    } else if (currentNavIndex.value >= 0 && currentNavIndex.value < activeSortieWaypoints.value.length) {
      targetCoord = activeSortieWaypoints.value[currentNavIndex.value];
    }
  }

  if (targetCoord && Array.isArray(targetCoord) && targetCoord.length >= 2) {
    let targetLat = targetCoord[0];
    let targetLng = targetCoord[1];
    let dist = calculateDistance(lat, lng, targetLat, targetLng);

    // Geofencing: Semak jika telah tiba dalam jarak 0.2 NM dari sasaran aktif (KRITERIA SELESAI)
    if (dist <= 0.2) {
      if (currentNavIndex.value === -1) {
        // Tiba di CSP
        cspStatus.value = 'completed';
        telahTibaCsp.value = true;
        playWaypointChime();
        console.log(`📍 Telah tiba di CSP (0.2 NM)! Beralih ke Waypoint seterusnya...`);
        if (activeSortieWaypoints.value.length > 0) {
          currentNavIndex.value = 0; // Teruskan ke WP 1
          targetCoord = activeSortieWaypoints.value[0];
          targetLat = targetCoord[0];
          targetLng = targetCoord[1];
          dist = calculateDistance(lat, lng, targetLat, targetLng);
        } else {
          isSearchCompleted.value = true;
        }
      } else if (currentNavIndex.value >= 0 && currentNavIndex.value < activeSortieWaypoints.value.length) {
        const curIdx = currentNavIndex.value;
        // Tiba di Waypoint semasa (BENAR-BENAR SELESAI <= 0.2 NM)
        if (!completedWpIndices.value.includes(curIdx)) {
          completedWpIndices.value.push(curIdx);
        }
        // Keluarkan dari senarai skip sekiranya sebelum ini ditanda skip
        skippedWpIndices.value = skippedWpIndices.value.filter(idx => idx !== curIdx);

        console.log(`📍 Telah tiba di WP ${curIdx + 1} (SELESAI)!`);
        playWaypointChime();

        if (curIdx + 1 < activeSortieWaypoints.value.length) {
          currentNavIndex.value += 1; // Teruskan ke WP berikutnya
          targetCoord = activeSortieWaypoints.value[currentNavIndex.value];
          targetLat = targetCoord[0];
          targetLng = targetCoord[1];
          dist = calculateDistance(lat, lng, targetLat, targetLng);
          console.log(`🧭 Sasaran baharu dikemas kini: WP ${currentNavIndex.value + 1}`);
        } else {
          // Telah selesai semua waypoint
          currentNavIndex.value = activeSortieWaypoints.value.length;
          isSearchCompleted.value = true;
          console.log(`🏁 Semua waypoint carian telah selesai dilalui!`);
        }
      }
    }

    if (!isSearchCompleted.value && targetCoord) {
      distanceToTarget.value = dist.toFixed(2);
      distanceToCsp.value = dist.toFixed(2); // Untuk data telemetri bilik gerakan
      courseToTarget.value = kiraBaringan(lat, lng, targetLat, targetLng);
      courseToCsp.value = courseToTarget.value;

      // Lukis atau kemaskini garisan panduan merah putus-putus ke sasaran aktif
      const linePoints = [[lat, lng], [targetLat, targetLng]];
      if (!cspLine.value && map.value) {
        cspLine.value = L.polyline(linePoints, { color: '#ef4444', weight: 2, dashArray: '5, 10' }).addTo(map.value);
      } else if (cspLine.value) {
        cspLine.value.setLatLngs(linePoints);
      }
    } else {
      distanceToTarget.value = '0.00';
      distanceToCsp.value = '0.00';
      if (cspLine.value && map.value) {
        map.value.removeLayer(cspLine.value);
        cspLine.value = null;
      }
    }
  } else {
    distanceToTarget.value = '0.00';
    distanceToCsp.value = '0.00';
    if (cspLine.value && map.value) {
      map.value.removeLayer(cspLine.value);
      cspLine.value = null;
    }
  }

  // 4. Tembak data satelit yang betul-betul live ini ke pangkalan data web
  hantarTelemetryKeWeb(lat, lng);
}

// PENGENDALIAN LUAR TALIAN & AUTO-SYNC
const simpanDataOffline = (point) => {
  offlineQueue.value.push(point);
  try {
    localStorage.setItem('sar_offline_tracks', JSON.stringify(offlineQueue.value));
  } catch (e) {}
};

const syncOfflineDataKeWeb = async () => {
  if (offlineQueue.value.length === 0 || isSyncingOffline.value || !isOnlineStatus.value) return;
  isSyncingOffline.value = true;
  try {
    const queueToFlush = [...offlineQueue.value];
    const { error } = await supabase.from('sru_track_history').insert(queueToFlush);
    if (!error) {
      offlineQueue.value = [];
      localStorage.removeItem('sar_offline_tracks');
      console.log(`✅ Berjaya sinkronasikan ${queueToFlush.length} jejak luar talian ke HQ!`);
    }
  } catch (err) {
    console.error("Gagal sync offline tracks:", err);
  } finally {
    isSyncingOffline.value = false;
  }
};

window.addEventListener('online', () => {
  isOnlineStatus.value = true;
  syncOfflineDataKeWeb();
});
window.addEventListener('offline', () => {
  isOnlineStatus.value = false;
});

const hantarTrackHistoryKeWeb = async (lat, lng) => {
  if (!selectedAsset.value || !selectedCaseId.value) return;

  const trackItem = {
    boat_id: selectedAsset.value,
    case_id: Number(selectedCaseId.value),
    latitude: Number(lat),
    longitude: Number(lng)
  };

  if (!isOnlineStatus.value) {
    simpanDataOffline(trackItem);
    return;
  }

  try {
    const { error } = await supabase.from('sru_track_history').insert([trackItem]);
    if (error) {
      console.error("Gagal simpan track history, simpan ke antrian offline:", error.message);
      simpanDataOffline(trackItem);
    }
  } catch (err) {
    simpanDataOffline(trackItem);
  }
};

const hantarTelemetryKeWeb = async (lat, lng) => {
  if (!selectedAsset.value) return;
  if (!isOnlineStatus.value) return;

  try {
    // 1. Padam kedudukan lama bot ini dari bilik gerakan bagi mengelakkan data bertimbun
    await supabase.from('sru_telemetry').delete().eq('boat_id', selectedAsset.value);

    // Tentukan nilai course untuk dihantar. Pastikan sentiasa numerik untuk mengelakkan ralat database.
    let courseValue = Number(botHeading.value);
    if (isNaN(courseValue) || botHeading.value === '-') {
      courseValue = 0; 
    }

    // 2. Masukkan koordinat satelit terbaharu yang betul-betul segar
    const { error } = await supabase.from('sru_telemetry').insert([
      {
        boat_id: selectedAsset.value,
        latitude: Number(lat),
        longitude: Number(lng),
        speed: parseFloat(botSpeed.value.toFixed(1)), 
        course: courseValue, // Gunakan nilai course yang telah ditentukan
        csp: parseFloat(distanceToTarget.value) || 0 
      }
    ]);

    if (error) {
      console.error("Gagal memancarkan isyarat GPS ke bilik gerakan HQ:", error.message);
    }
  } catch (err) {
    console.error("Ralat telemetri:", err);
  }
};

// =========================================================================
// CIRI TAKTIKAL: SIGHTING / PENEMUAN OBJEK (POI)
// =========================================================================
const rekodSighting = async () => {
  if (currentLat.value === 0 && currentLng.value === 0) {
    alert("⚠️ Koordinat GPS belum dikesan!");
    return;
  }

  const now = new Date();
  const masaString = now.toLocaleTimeString('ms-MY', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  const sightingBaru = {
    id: Date.now(),
    type: sightingType.value,
    remarks: sightingRemarks.value.trim() || 'Tiada nota tambahan',
    lat: currentLat.value,
    lng: currentLng.value,
    time: masaString,
    boat_id: selectedAsset.value,
    case_id: Number(selectedCaseId.value)
  };

  senaraiSightings.value.unshift(sightingBaru);

  // Lukis marker penemuan pada peta Leaflet
  if (map.value) {
    const sIcon = L.divIcon({
      className: 'sighting-marker-icon',
      html: `<div style="background: #ea580c; color: white; border: 2px solid #ffffff; border-radius: 50%; width: 26px; height: 26px; display: flex; align-items: center; justify-content: center; font-size: 13px; box-shadow: 0 0 12px #f97316; animation: pulse 1.5s infinite;">
               📍
             </div>`,
      iconSize: [26, 26],
      iconAnchor: [13, 13]
    });

    const sMarker = L.marker([currentLat.value, currentLng.value], { icon: sIcon })
      .addTo(map.value)
      .bindPopup(`<div style="color: #0f172a; font-size: 12px; font-family: sans-serif;">
                    <strong style="color: #ea580c;">📍 PENEMUAN: ${sightingBaru.type}</strong><br/>
                    <span>Masa: ${masaString}</span><br/>
                    <span>Aset: ${selectedAsset.value}</span><br/>
                    <span>Pos: ${formatCoordinate(currentLat.value, true)} ${formatCoordinate(currentLng.value, false)}</span><br/>
                    <em>Nota: ${sightingBaru.remarks}</em>
                  </div>`);
    
    sMarker.openPopup();
    sightingMarkersList.value.push(sMarker);
  }

  // Hantar notifikasi penemuan ke chat HQ
  try {
    await supabase.from('sar_messages').insert([
      {
        case_id: Number(selectedCaseId.value),
        sender: selectedAsset.value,
        message: `📍 [PENEMUAN/SIGHTING] ${sightingBaru.type} pada ${formatCoordinate(currentLat.value, true)} ${formatCoordinate(currentLng.value, false)} (${masaString}). Nota: ${sightingBaru.remarks}`,
        chat_type: 'local'
      }
    ]);
  } catch (err) {
    console.error("Gagal hantar notifikasi sighting:", err);
  }

  playWaypointChime();
  sightingRemarks.value = '';
  paparSightingModal.value = false;
};

// =========================================================================
// CIRI TAKTIKAL: MOB (MAN OVERBOARD) / KECEMASAN MAYDAY
// =========================================================================
const aktifkanMOB = async () => {
  if (currentLat.value === 0 && currentLng.value === 0) {
    alert("⚠️ Koordinat GPS belum dikesan!");
    return;
  }

  mobCoord.value = [currentLat.value, currentLng.value];
  const now = new Date();
  mobTime.value = now.toLocaleTimeString('ms-MY', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  isMobActive.value = true;

  if (map.value) {
    const mobIcon = L.divIcon({
      className: 'mob-marker-icon',
      html: `<div style="background: #dc2626; color: white; border: 2px solid #ffffff; border-radius: 50%; width: 32px; height: 32px; display: flex; align-items: center; justify-content: center; font-size: 16px; font-weight: bold; box-shadow: 0 0 20px #ef4444; animation: pulse 0.8s infinite;">
               🆘
             </div>`,
      iconSize: [32, 32],
      iconAnchor: [16, 16]
    });

    if (mobMarkerInstance) map.value.removeLayer(mobMarkerInstance);
    if (mobCircleInstance) map.value.removeLayer(mobCircleInstance);

    mobMarkerInstance = L.marker(mobCoord.value, { icon: mobIcon })
      .addTo(map.value)
      .bindPopup(`<strong style="color: #dc2626; font-size: 13px;">🚨 MAN OVERBOARD (MOB)</strong><br/>Masa: ${mobTime.value}<br/>Pos: ${formatCoordinate(mobCoord.value[0], true)} ${formatCoordinate(mobCoord.value[1], false)}`)
      .openPopup();

    // Zon radius amaran MOB (0.2 NM = ~370 meter)
    mobCircleInstance = L.circle(mobCoord.value, {
      radius: 370,
      color: '#dc2626',
      fillColor: '#ef4444',
      fillOpacity: 0.25,
      weight: 2,
      dashArray: '4, 4'
    }).addTo(map.value);
  }

  playMobAlarm();

  // Hantar siaran kecemasan ke HQ
  try {
    await supabase.from('sar_messages').insert([
      {
        case_id: Number(selectedCaseId.value),
        sender: selectedAsset.value,
        message: `🚨 [KECEMASAN MAYDAY / MOB] Aset ${selectedAsset.value} mengaktifkan MOB pada kedudukan ${formatCoordinate(mobCoord.value[0], true)} ${formatCoordinate(mobCoord.value[1], false)} (${mobTime.value})!`,
        chat_type: 'local'
      }
    ]);
  } catch (err) {
    console.error("Gagal hantar amaran MOB:", err);
  }
};

const batalkanMOB = () => {
  isMobActive.value = false;
  mobCoord.value = null;
  if (mobMarkerInstance && map.value) {
    map.value.removeLayer(mobMarkerInstance);
    mobMarkerInstance = null;
  }
  if (mobCircleInstance && map.value) {
    map.value.removeLayer(mobCircleInstance);
    mobCircleInstance = null;
  }
  paparMobModal.value = false;
};

// =========================================================================
// CIRI TAKTIKAL: KAWALAN MANUAL WAYPOINT & REVERSE TRACK
// =========================================================================
const langkauKeWpSeterusnya = () => {
  if (isSearchCompleted.value) return;
  
  if (currentNavIndex.value === -1) {
    // Tandakan CSP sebagai SKIP jika belum diselesaikan
    if (cspStatus.value !== 'completed') {
      cspStatus.value = 'skipped';
    }
    telahTibaCsp.value = true;
    if (activeSortieWaypoints.value.length > 0) {
      currentNavIndex.value = 0;
    } else {
      isSearchCompleted.value = true;
    }
  } else if (currentNavIndex.value >= 0 && currentNavIndex.value < activeSortieWaypoints.value.length) {
    const curIdx = currentNavIndex.value;
    // Tandakan WP semasa sebagai SKIP jika belum diselesaikan
    if (!completedWpIndices.value.includes(curIdx) && !skippedWpIndices.value.includes(curIdx)) {
      skippedWpIndices.value.push(curIdx);
    }

    if (curIdx < activeSortieWaypoints.value.length - 1) {
      currentNavIndex.value += 1;
    } else {
      currentNavIndex.value = activeSortieWaypoints.value.length;
      isSearchCompleted.value = true;
    }
  }
  playWaypointChime();
};

const pilihWpManual = (index) => {
  if (index >= 0 && index < activeSortieWaypoints.value.length) {
    currentNavIndex.value = index;
    isSearchCompleted.value = false;
    telahTibaCsp.value = true;
    playWaypointChime();
  }
};



const toggleNightMode = () => {
  isNightMode.value = !isNightMode.value;
};

// Haversine formula untuk mengira jarak antara dua titik latitud/longitud
const calculateDistance = (lat1, lon1, lat2, lon2) => {
  const R = 6371e3; // jejari bumi dalam meter
  const φ1 = lat1 * Math.PI / 180; // latitud, longitud dalam radian
  const φ2 = lat2 * Math.PI / 180;
  const Δφ = (lat2 - lat1) * Math.PI / 180;
  const Δλ = (lon2 - lon1) * Math.PI / 180;

  const a = Math.sin(Δφ / 2) * Math.sin(Δφ / 2) +
            Math.cos(φ1) * Math.cos(φ2) *
            Math.sin(Δλ / 2) * Math.sin(Δλ / 2);
  const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));

  const distanceMeters = R * c; // dalam meter
  return distanceMeters / 1852; // Tukar kepada Batu Nautika (1 NM = 1852 meter)
};

// Fungsi mengira baringan (course) dari koordinat A ke B
const kiraBaringan = (lat1, lon1, lat2, lon2) => {
  const φ1 = lat1 * Math.PI / 180;
  const φ2 = lat2 * Math.PI / 180;
  const Δλ = (lon2 - lon1) * Math.PI / 180;

  const y = Math.sin(Δλ) * Math.cos(φ2);
  const x = Math.cos(φ1) * Math.sin(φ2) - Math.sin(φ1) * Math.cos(φ2) * Math.cos(Δλ);
  const θ = Math.atan2(y, x);
  const bearing = (θ * 180 / Math.PI + 360) % 360;
  return Math.round(bearing);
};

const recenterMap = () => {
  isFollowing.value = true;
  fokusLokasiOffset();
};

const fokusLokasiOffset = () => {
  if (map.value && currentLat.value !== 0) {
    const zoom = 15;
    // Kita kira titik koordinat baru supaya marker GPS berada di bahagian bawah skrin.
    const userPoint = map.value.project([currentLat.value, currentLng.value], zoom);
    const shiftedPoint = userPoint.subtract([0, 100]); // Anjakkan pusat peta ke atas 100px
    const shiftedLatLng = map.value.unproject(shiftedPoint, zoom);
    map.value.flyTo(shiftedLatLng, zoom);
  }
};

const toggleWaypointList = async () => {
  paparWaypointList.value = !paparWaypointList.value;
  paparConfirmStop.value = false;
  paparChat.value = false; // Tutup chat jika buka waypoint list
  if (paparWaypointList.value) await muatTurunDetailPelanSAROPS();
};
const hentiTracking = async () => {
  if (watchId.value) {
    // Semak jika watchId adalah dari navigator.geolocation (nombor) atau Capacitor (objek dengan id)
    if (typeof watchId.value === 'number') { // Web Geolocation watchId is a number
      navigator.geolocation.clearWatch(watchId.value);
    } else if (watchId.value && watchId.value.id) { // Capacitor watchId is an object with an 'id' property
      await Geolocation.clearWatch({ id: watchId.value.id });
    }
  }
  
  await supabase.from('sru_telemetry').delete().eq('boat_id', selectedAsset.value)
  padamSesiAktif();

  isTracking.value = false
  telahTibaCsp.value = false
  currentNavIndex.value = -1
  isSearchCompleted.value = false
  cspStatus.value = 'pending'
  completedWpIndices.value = []
  skippedWpIndices.value = []
  activeSortieWaypoints.value = []
  activeCspCoord.value = null
  targetCspLat.value = null; // Reset CSP target
  targetCspLng.value = null; // Reset CSP target
  distanceToTarget.value = '0.00'
  courseToTarget.value = '-'
  distanceToCsp.value = '0.00'
  courseToCsp.value = '-'

  // RESET TRACK HISTORY
  trackHistoryCoordinates.value = [];
  if (trackPolylineInstance && map.value) {
    map.value.removeLayer(trackPolylineInstance);
    trackPolylineInstance = null;
  }

  // Bersihkan penanda waypoint & CSP dari peta
  wpMarkersList.value.forEach(m => {
    if (map.value && m) map.value.removeLayer(m);
  });
  wpMarkersList.value = [];

  // Bersihkan penanda Sighting
  sightingMarkersList.value.forEach(m => {
    if (map.value && m) map.value.removeLayer(m);
  });
  sightingMarkersList.value = [];

  // Bersihkan penanda MOB
  if (mobMarkerInstance && map.value) {
    map.value.removeLayer(mobMarkerInstance);
    mobMarkerInstance = null;
  }
  if (mobCircleInstance && map.value) {
    map.value.removeLayer(mobCircleInstance);
    mobCircleInstance = null;
  }
  isMobActive.value = false;
  mobCoord.value = null;
  paparSightingModal.value = false;
  paparMobModal.value = false;

  if (cspLine.value) { map.value.removeLayer(cspLine.value); cspLine.value = null }
  if (cspMarker.value) { map.value.removeLayer(cspMarker.value); cspMarker.value = null }
  if (userMarker.value) { map.value.removeLayer(userMarker.value); userMarker.value = null }
  map.value = null

  if (chatPollingTimer) {
    clearInterval(chatPollingTimer)
    chatPollingTimer = null
  }
  if (chatChannel) {
    supabase.removeChannel(chatChannel)
    chatChannel = null
  }
  toastNotifikasi.value.papar = false

  currentScreen.value = 'setup'
}

// =========================================================================
// LOGIK FASA D: TAKTICAL REALTIME CHAT & NOTIFIKASI KECEMASAN MERENTAS PERANTI
// =========================================================================
let chatChannel = null
let chatPollingTimer = null

const prosesMesejMasuk = (msgObj) => {
  if (!msgObj || !msgObj.id) return
  if (Number(msgObj.case_id) !== Number(selectedCaseId.value)) return
  if (msgObj.chat_type && msgObj.chat_type !== 'local') return

  const mesejWujud = senaraiMesej.value.some(msg => msg.id === msgObj.id)
  if (!mesejWujud) {
    senaraiMesej.value.push(msgObj)
    autoScrollChat()

    // Semak sama ada mesej ini dihantar oleh peranti / aset LAIN
    const isFromOtherDevice = (msgObj.sender || '').trim().toLowerCase() !== (selectedAsset.value || '').trim().toLowerCase()

    if (isFromOtherDevice) {
      if (!paparChat.value) {
        unreadCount.value++
      }

      const text = msgObj.message || ''

      // 1. KES MOB (MAN OVERBOARD) DARI PERANTI LAIN
      if (text.includes('[KECEMASAN MAYDAY / MOB]') || text.includes('MOB') || text.includes('MAYDAY')) {
        playMobAlarm()
        tunjukToastNotifikasi('mob', '🚨', `AMARAN MOB: ${msgObj.sender}`, text, 12000)
        hantarNotifikasiOS(`🚨 AMARAN MOB: ${msgObj.sender}`, text)
      } 
      // 2. KES SIGHTING / PENEMUAN DARI PERANTI LAIN
      else if (text.includes('[PENEMUAN/SIGHTING]') || text.includes('PENEMUAN') || text.includes('SIGHTING')) {
        playSightingSound()
        tunjukToastNotifikasi('sighting', '📍', `PENEMUAN: ${msgObj.sender}`, text, 8000)
        hantarNotifikasiOS(`📍 Penemuan Baru dari ${msgObj.sender}`, text)
      } 
      // 3. MESEJ CHAT BIASA DARI PERANTI LAIN
      else {
        playIncomingMessageSound()
        tunjukToastNotifikasi('mesej', '💬', `Mesej dari ${msgObj.sender}`, text, 5000)
        hantarNotifikasiOS(`💬 ${msgObj.sender}`, text)
      }
    }
  }
}

const muatTurunMesej = async () => {
  const { data } = await supabase
    .from('sar_messages')
    .select('*')
    .eq('case_id', Number(selectedCaseId.value))
    .eq('chat_type', 'local')
    .order('created_at', { ascending: true })
    
  if (data && data.length > 0) {
    senaraiMesej.value = data
  }
  autoScrollChat()
}

const hantarMesej = async () => {
  if (!mesejBaharu.value.trim()) return

  const customMsg = {
    case_id: Number(selectedCaseId.value),
    sender: selectedAsset.value,
    message: mesejBaharu.value.trim(),
    chat_type: 'local'
  }

  const { data, error } = await supabase.from('sar_messages').insert([customMsg]).select().single()
  if (!error) {
    mesejBaharu.value = ''
    if (data) {
      senaraiMesej.value.push(data)
      autoScrollChat()
    }
  }
}

const dengarChatLive = () => {
  if (chatChannel) {
    supabase.removeChannel(chatChannel)
    chatChannel = null
  }

  mintaIzinNotifikasi()

  // 1. Langgan Saluran Realtime Supabase (Postgres Changes & Broadcast)
  chatChannel = supabase
    .channel(`sar-messages-case-${selectedCaseId.value}`)
    .on('postgres_changes', { event: 'INSERT', schema: 'public', table: 'sar_messages' }, payload => {
      if (payload && payload.new) {
        prosesMesejMasuk(payload.new)
      }
    })
    .subscribe((status) => {
      console.log(`📡 Status Saluran Realtime M-SRU NAV: ${status}`)
    })

  // 2. Polling sandaran (Fallback Poller) setiap 2.5 saat bagi memastikan notifikasi sentiasa diterima tanpa tercicir
  if (chatPollingTimer) clearInterval(chatPollingTimer)
  chatPollingTimer = setInterval(async () => {
    if (currentScreen.value === 'map' && selectedCaseId.value) {
      const { data } = await supabase
        .from('sar_messages')
        .select('*')
        .eq('case_id', Number(selectedCaseId.value))
        .eq('chat_type', 'local')
        .order('created_at', { ascending: true })

      if (data && data.length > 0) {
        data.forEach(msg => prosesMesejMasuk(msg))
      }
    }
  }, 2500)
}

const autoScrollChat = () => {
  nextTick(() => {
    const el = document.querySelector('.chat-box')
    if (el) el.scrollTop = el.scrollHeight
  })
}
</script>

<style scoped>
@keyframes slideIn { from { transform: translateX(100%); } to { transform: translateX(0); } }
.chat-box::-webkit-scrollbar, .scroll-taktikal::-webkit-scrollbar { width: 4px; }
.chat-box::-webkit-scrollbar-thumb, .scroll-taktikal::-webkit-scrollbar-thumb { background: #60a5fa; border-radius: 2px; }

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
.fade-in-content {
  animation: fadeIn 0.8s ease-out forwards;
}

@keyframes wave {
  0%, 100% { transform: translateY(0) skewX(-2deg); }
  50% { transform: translateY(-5px) skewX(2deg); }
}
.wave-text span {
  display: inline-block;
  animation: wave 2s ease-in-out infinite;
  /* Rupa Chrome/Metallic */
  background: linear-gradient(to bottom, #ffffff 0%, #cbd5e1 45%, #475569 50%, #94a3b8 55%, #ffffff 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  /* Kesan 3D Extrusion */
  filter: drop-shadow(1px 1px 0px #cbd5e1) 
          drop-shadow(2px 2px 0px #475569) 
          drop-shadow(4px 4px 6px rgba(0,0,0,0.7));
}

/* Animasi Slide Up untuk SAP & Message */
.slide-up-enter-active, .slide-up-leave-active {
  transition: all 0.3s ease-out;
}
.slide-up-enter-from, .slide-up-leave-to {
  transform: translateY(30px);
  opacity: 0;
}

/* Animasi Pop untuk Confirmation */
.pop-enter-active, .pop-leave-active {
  transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.pop-enter-from, .pop-leave-to {
  transform: scale(0.9);
  opacity: 0;
}

/* MOD MALAM TAKTIKAL (MARITIME RED NIGHT VISION) */
.night-vision {
  background-color: #050000 !important;
}
.night-vision :deep(.leaflet-tile-pane) {
  filter: invert(100%) hue-rotate(180deg) brightness(80%) contrast(130%) sepia(100%) saturate(350%) hue-rotate(-50deg);
}
.night-vision div[style*="background: rgba(15, 23, 42"] {
  background: rgba(20, 3, 3, 0.85) !important;
  border-color: #7f1d1d !important;
}
</style>