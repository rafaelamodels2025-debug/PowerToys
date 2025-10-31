<p align="center">
    <picture>
      <source media="(prefers-color-scheme: light)" srcset="./doc/images/readme/pt-hero.light.png" />
      <img src="./doc/images/readme/pt-hero.dark.png" />
  </picture>
</p>
<h1 align="center">
  <span>Microsoft PowerToys</span>
</h1>

<h3 align="center">
  <a href="#-installation">Installation</a>
  <span> . </span>
  <a href="https://aka.ms/powertoys-docs">Documentation</a>
  <span> . </span>
  <a href="https://aka.ms/powertoys-releaseblog">Blog</a>
  <span> . </span>
  <a href="#-whats-new">Release notes</a>
</h3>
<br/><br/>
Microsoft PowerToys is a collection of utilities that help you customize Windows and streamline everyday tasks.
<br/><br/>

|   |   |   |
|---|---|---|
| [<img src="doc/images/icons/AdvancedPaste.png" alt="Advanced Paste icon" height="16"> Advanced Paste](https://aka.ms/PowerToysOverview_AdvancedPaste) | [<img src="doc/images/icons/Always%20On%20Top.png" alt="Always on Top icon" height="16"> Always on Top](https://aka.ms/PowerToysOverview_AoT) | [<img src="doc/images/icons/Awake.png" alt="Awake icon" height="16"> Awake](https://aka.ms/PowerToysOverview_Awake) |
| [<img src="doc/images/icons/Color%20Picker.png" alt="Color Picker icon" height="16"> Color Picker](https://aka.ms/PowerToysOverview_ColorPicker) | [<img src="doc/images/icons/Command%20Not%20Found.png" alt="Command Not Found icon" height="16"> Command Not Found](https://aka.ms/PowerToysOverview_CmdNotFound) | [<img src="doc/images/icons/Command Palette.png" alt="Command Palette icon" height="16"> Command Palette](https://aka.ms/PowerToysOverview_CmdPal) |
| [<img src="doc/images/icons/Crop%20And%20Lock.png" alt="Crop and Lock icon" height="16"> Crop And Lock](https://aka.ms/PowerToysOverview_CropAndLock) | [<img src="doc/images/icons/Environment%20Manager.png" alt="Environment Variables icon" height="16"> Environment Variables](https://aka.ms/PowerToysOverview_EnvironmentVariables) | [<img src="doc/images/icons/FancyZones.png" alt="FancyZones icon" height="16"> FancyZones](https://aka.ms/PowerToysOverview_FancyZones) |
| [<img src="doc/images/icons/File%20Explorer%20Preview.png" alt="File Explorer Add-ons icon" height="16"> File Explorer Add-ons](https://aka.ms/PowerToysOverview_FileExplorerAddOns) | [<img src="doc/images/icons/File%20Locksmith.png" alt="File Locksmith icon" height="16"> File Locksmith](https://aka.ms/PowerToysOverview_FileLocksmith) | [<img src="doc/images/icons/Host%20File%20Editor.png" alt="Hosts File Editor icon" height="16"> Hosts File Editor](https://aka.ms/PowerToysOverview_HostsFileEditor) |
| [<img src="doc/images/icons/Image%20Resizer.png" alt="Image Resizer icon" height="16"> Image Resizer](https://aka.ms/PowerToysOverview_ImageResizer) | [<img src="doc/images/icons/Keyboard%20Manager.png" alt="Keyboard Manager icon" height="16"> Keyboard Manager](https://aka.ms/PowerToysOverview_KeyboardManager) | [<img src="doc/images/icons/Light Switch.png" alt="Light Switch icon" height="16"> Light Switch](https://aka.ms/PowerToysOverview_LightSwitch) |
| [<img src="doc/images/icons/Find My Mouse.png" alt="Mouse Utilities icon" height="16"> Mouse Utilities](https://aka.ms/PowerToysOverview_MouseUtilities) | [<img src="doc/images/icons/MouseWithoutBorders.png" alt="Mouse Without Borders icon" height="16"> Mouse Without
import random  # precisa importar primeiro

# Base de dados de posts do usuário (cores, estilos, tipos)
user_creations = [
    {"type": "logo", "color": "gold", "style": "minimal"},
    {"type": "post", "color": "pink", "style": "vibrant"},
    {"type": "banner", "color": "blue", "style": "clean"},
]

# Função simples de aprendizado (sugestão de melhorias)
def angel_suggestion(user_creations):
    preferred_colors = [c["color"] for c in user_creations]
    preferred_styles = [c["style"] for c in user_creations]

    # Sugestão aleatória baseada no histórico
    suggestion = {
        "type": random.choice(["post", "logo", "banner"]),
        "color": random.choice(preferred_colors),
        "style": random.choice(preferred_styles),
        "hashtag": "#DesignByAngel",
        "caption": "Criação top inspirada no seu estilo!"
    }
    return suggestion

# Demonstração# anjodaguarda_full.py
"""
Protótipo integrado: captura de sensores, buffer persistente, decisão de alerta,
geração de variantes de imagem (Ultra Red / Violeta), número efêmero e envio stub.
Substitua stubs por integração real (WhatsApp Business API, provider satelital, TTS).
"""

import os
import json
import time
import random
import hashlib
import zipfile
import uuid
from datetime import datetime, timezone
from collections import deque
from typing import Dict

# Imagem: Pillow
from PIL import Image, ImageEnhance, ImageOps

# ---------------- Config ----------------
LOCAL_STORAGE = "./anjodaguarda_buffer/"
os.makedirs(LOCAL_STORAGE, exist_ok=True)

# Simulação de disponibilidade de canais (ajuste durante testes)
network_state = {
    "internet": True,
    "gsm": True,
    "satellite": True,
    "lora": False,
    "bluetooth_mesh": False
}

# ---------------- Utilitários ----------------
def now_str():
    return datetime.now(timezone.utc).isoformat()

def safe_hash(s: bytes) -> str:
    return hashlib.sha256(s).hexdigest()

# ---------------- Buffer persistente ----------------
class PersistentQueue:
    def __init__(self, folder=LOCAL_STORAGE):
        self.folder = folder
        self.queue = deque()
        self._load_from_disk()

    def _load_from_disk(self):
        for fname in sorted(os.listdir(self.folder)):
            if fname.endswith(".buf"):
                path = os.path.join(self.folder, fname)
                try:
                    with open(path, "rb") as f:
                        raw = f.read()
                    item = json.loads(raw.decode("utf-8"))
                    self.queue.append((item, path))
                except Exception:
                    continue

    def push(self, item: dict):
        item["_meta"] = {"created_at": now_str()}
        raw = json.dumps(item, ensure_ascii=False).encode("utf-8")
        ts = int(time.time()*1000)
        fname = f"item_{ts}_{random.randint(0,9999)}.buf"
        path = os.path.join(self.folder, fname)
        with open(path, "wb") as f:
            f.write(raw)
        self.queue.append((item, path))
        return path

    def pop(self):
        if not self.queue:
            return None
        item, path = self.queue.popleft()
        try:
            if os.path.exists(path):
                os.remove(path)
        except Exception:
            pass
        return item

    def peek_all(self):
        return [it for it, _ in list(self.queue)]

buffer = PersistentQueue()

# ----------------- Sensores Simulados / Estimator -----------------
def get_gps():
    if random.random() < 0.6:
        return {"lat": -6.8 + random.uniform(-0.002,0.002),
                "lon": -38.1 + random.uniform(-0.002,0.002),
                "accuracy_m": 5 + random.random()*10}
    return None

class PositionEstimator:
    def __init__(self):
        self.estimate = None

    def update_with_gps(self, gps):
        if gps:
            self.estimate = (gps["lat"], gps["lon"])
            return self.estimate
        # fallback - tiny random walk (simulação)
        if self.estimate:
            lat, lon = self.estimate
            lat += random.uniform(-1,1)*1e-5
            lon += random.uniform(-1,1)*1e-5
            self.estimate = (lat, lon)
            return self.estimate
        return None

estimator = PositionEstimator()

# ----------------- Senders (stubs) -----------------
def send_via_internet(payload: dict):
    if not network_state["internet"]:
        raise ConnectionError("Internet indisponível")
    print("[ENVIANDO INTERNET]", payload.get("tipo"), payload.get("event_id"))
    return True

def send_via_gsm_sms(payload: dict):
    if not network_state["gsm"]:
        raise ConnectionError("GSM indisponível")
    print("[ENVIANDO GSM/SMS]", payload.get("tipo"), payload.get("event_id"))
    return True

def send_via_satellite_real(payload: dict):
    if not network_state["satellite"]:
        raise ConnectionError("Satélite indisponível")
    print("[ENVIANDO SATÉLITE REAL]", payload.get("tipo"), payload.get("event_id"))
    return True

def send_via_lora(payload: dict):
    if not network_state["lora"]:
        raise ConnectionError("LoRa indisponível")
    print("[ENVIANDO LORA]", payload.get("tipo"), payload.get("event_id"))
    return True

def send_via_mesh(payload: dict):
    if not network_state["bluetooth_mesh"]:
        raise ConnectionError("Mesh indisponível")
    print("[ENVIANDO MESH]", payload.get("tipo"), payload.get("event_id"))
    return True

SENDERS = [send_via_satellite_real, send_via_internet, send_via_gsm_sms, send_via_lora, send_via_mesh]

# ----------------- Forensics / pacote -----------------
def create_forensic_package(event: dict, attachments: list, output_path="./package.zip"):
    tmpdir = "./tmp_pkg"
    os.makedirs(tmpdir, exist_ok=True)
    ev_json_path = os.path.join(tmpdir, "event.json")
    with open(ev_json_path, "w", encoding="utf-8") as f:
        json.dump(event, f, ensure_ascii=False, indent=2)
    for a in attachments:
        src = a.get("path")
        if src and os.path.exists(src):
            dst = os.path.join(tmpdir, os.path.basename(src))
            with open(src, "rb") as fr, open(dst, "wb") as fw:
                fw.write(fr.read())
    zipf = zipfile.ZipFile(output_path, 'w', zipfile.ZIP_DEFLATED)
    zipf.write(ev_json_path, arcname="event.json")
    zipf.close()
    return output_path

# ----------------- Core capture & retries -----------------
def capturar_evento(tipo: str, extra: dict):
    gps = get_gps()
    pos = estimator.update_with_gps(gps)
    evento = {
        "event_id": str(uuid.uuid4()),
        "tipo": tipo,
        "ts": now_str(),
        "pos": {"lat": pos[0], "lon": pos[1]} if pos else None,
        "gps_raw": gps,
        "extra": extra
    }
    path = buffer.push(evento)
    print(f"[CAPTURADO] {tipo} -> {path}")
    tentar_envio_retries()
    return evento

def tentar_envio_retries(max_attempts_per_item=3):
    items = buffer.peek_all()
    for item in items:
        attempts = 0
        sent = False
        while attempts < max_attempts_per_item and not sent:
            for sender in SENDERS:
                try:
                    sender(item)
                    sent = True
                    break
                except ConnectionError:
                    continue
            attempts += 1
            if not sent:
                backoff = 2 ** attempts
                print(f"[RETRY] falhou, backoff {backoff}s")
                time.sleep(backoff)
        if sent:
            buffer.pop()
            print("[OK] enviado e removido do buffer")
        else:
            print("[NOK] permanece no buffer")

# ----------------- Image utilities (Pillow) -----------------
def open_img(path):
    img = Image.open(path).convert("RGBA")
    return img

def resize_square(img, size=512):
    img = ImageOps.contain(img, (size, size))
    canvas = Image.new("RGBA", (size, size), (0,0,0,0))
    x = (size - img.width)//2
    y = (size - img.height)//2
    canvas.paste(img, (x,y), img)
    return canvas

def tint_image(img, tint_color=(255,0,0), strength=0.25):
    color_layer = Image.new("RGBA", img.size, tint_color + (int(255*strength),))
    blended = Image.alpha_composite(img, color_layer)
    enhancer = ImageEnhance.Brightness(blended)
    blended = enhancer.enhance(1.05)
    return blended

def generate_variants(input_path):
    img = open_img(input_path)
    img512 = resize_square(img, 512)
    red = tint_image(img512, tint_color=(255,40,40), strength=0.20)
    vio = tint_image(img512, tint_color=(150,50,200), strength=0.20)
    azul = tint_image(img512, tint_color=(30,50,180), strength=0.20)
    red_out = "anjinho_ultrared.png"
    vio_out = "anjinho_violeta.png"
    azul_out = "anjinho_azul.png"
    red.save(red_out, format="PNG")
    vio.save(vio_out, format="PNG")
    azul.save(azul_out, format="PNG")
    print("Gerados:", red_out, vio_out, azul_out)
    # Comandos para converter para webp (use no terminal)
    print("\nPara converter para WEBP (terminal):")
    print("cwebp -q 95 anjinho_ultrared.png -o anjinho_ultrared.webp")
    print("cwebp -q 95 anjinho_violeta.png -o anjinho_violeta.webp")
    print("cwebp -q 95 anjinho_azul.png -o anjinho_azul.webp")
    return [red_out, vio_out, azul_out]

# ----------------- Number & messaging stubs -----------------
def gerar_numero_efemero():
    return f"+000{random.randint(100000,999999)}"

def enviar_whatsapp(mensagem, imagem=None, numero=None):
    # TODO: integrar com WhatsApp Business API provider
    print(f"[WA SEND] {numero} | {mensagem} | asset={imagem}")

def enviar_SMS_backup(mensagem, numero=None):
    # TODO: integrar com Twilio/Nexmo
    print(f"[SMS SEND] {numero} | {mensagem}")

def apagar_mensagens_whatsapp(numero):
    # TODO: provider-specific API to delete messages (if supported)
    print(f"[APAGAR WA] {numero} (stub)")

def apagar_mensagens_SMS(numero):
    # TODO: provider-specific
    print(f"[APAGAR SMS] {numero} (stub)")

# ----------------- High-level alert API -----------------
def gerar_imagem_IA(tipo_alerta):
    if tipo_alerta == "Violeta Power":
        return "anjinho_violeta.webp"
    if tipo_alerta == "Blue Power":
        return "anjinho_azul.webp"
    return "anjinho_ultrared.webp"

def enviar_alerta(tipo_alerta, extra=None):
    imagem = gerar_imagem_IA(tipo_alerta)
    mensagem = f"Alerta {tipo_alerta} acionado! — Anjinho da Guarda"
    numero_efemero = gerar_numero_efemero()
    enviar_whatsapp(mensagem, imagem, numero_efemero)
    enviar_SMS_backup(mensagem, numero_efemero)
    return numero_efemero

def receber_alerta(tipo_alerta, extra=None):
    if tipo_alerta in ["Violeta Power", "Blue Power", "Ultra Red Power"]:
        evt = capturar_evento(tipo_alerta, extra or {})
        num = enviar_alerta(tipo_alerta, extra)
        return evt, num
    else:
        print("[WARN] alerta desconhecido:", tipo_alerta)
        return None, None

def encerrar_caso(case_id):
    mensagem_final = "Pela ética, peço que a vítima não saiba que fui eu que passei as informações. — Anjinho da Guarda"
    numero_efemero = gerar_numero_efemero()
    enviar_whatsapp(mensagem_final, None, numero_efemero)
    apagar_mensagens_whatsapp(numero_efemero)
    apagar_mensagens_SMS(numero_efemero)
    limpar_dados_temporarios(case_id)

def limpar_dados_temporarios(case_id):
    print(f"[CLEANUP] case {case_id} cleaned (stub)")

# ----------------- Sensor fusion handler -----------------
def compute_confidence(scores: Dict[str, float], weights=None) -> float:
    if weights is None:
        weights = {'vision':0.4, 'radar':0.3, 'audio':0.2, 'ble':0.1}
    total = 0.0
    for k,w in weights.items():
        s = float(scores.get(k, 0.0))
        total += s * w
    return total

def handle_sensor_event(sensor_payload: dict):
    scores = sensor_payload.get("scores", {})
    confidence = compute_confidence(scores)
    high_sensors = sum(1 for v in scores.values() if v > 0.5)
    summary = sensor_payload.get("summary","").lower()
    print(f"[SENSOR] confidence={confidence:.2f} summary='{summary}'")

    if confidence >= 0.90 and high_sensors >= 2:
        # ameaça grave
        extra = {"source":"sensor_gateway","summary":sensor_payload.get("summary"), "scores":scores, "pos":sensor_payload.get("pos")}
        evt, num = receber_alerta("Ultra Red Power", extra)
        if scores.get("vision",0) >= 0.8:
            print("[ACTION] ativar corda energetica (condicional) - stub")
    elif confidence >= 0.60 and high_sensors >= 2:
        tipo = "Violeta Power" if any(k in summary for k in ["terreiro","relig","sagrado","kimbanda","umbanda","intoler"]) else "Blue Power"
        evt, num = receber_alerta(tipo, {"source":"sensor_gateway","summary":sensor_payload.get("summary"), "scores":scores})
    elif confidence >= 0.40:
        print("[MONITOR] aumentar vigilância e logar evento")
    else:
        print("[IGNORAR] baixa confiança")

# ----------------- Exemplo de execução -----------------
if __name__ == "__main__":
    print("=== AnjoDaGuarda Full Prototype ===")
    # gerar variantes da sua imagem (substitua 'anjinho_input.png' pelo seu arquivo)
    # generate_variants("anjinho_input.png")  # descomente quando tiver o arquivo

    # Exemplo de sensor payload recebido do gateway
    example_payload = {
      "event_id": str(uuid.uuid4()),
      "source": "sensor_gateway_001",
      "tipo": "sensor_alert",
      "ts": now_str(),
      "scores": {"vision":0.9, "radar":0.82, "audio":0.7, "ble":0.0},
      "summary": "Homem com objeto potencialmente perigoso se aproximando da entrada",
      "pos": {"lat": -6.8001, "lon": -38.101}
    }

    handle_sensor_event(example_payload)

    # Simular alert manual
    receber_alerta("Ultra Red Power", {"descricao":"Gritos e objeto quebrando", "owner_id":"dona_001"})
    time.sleep(0.5)
    receber_alerta("Violeta Power", {"descricao":"Ameaças em terreiro", "owner_id":"dona_002"})
    time.sleep(0.5)
    receber_alerta("Blue Power", {"descricao":"Situação de atenção", "owner_id":"dona_003"})SENDERS = [send_via_satellite_real, send_via_internet, send_via_gsm_sms, send_via_lora, send_via_mesh]anjodaguarda/
├─ README.md
├─ requirements.txt
├─ src/
│  ├─ sensors.py                # leitura GPS, IMU, sensor de toque (pulseira), sensor de remoção
│  ├─ vision.py                 # detecção de pessoa, zoom, descrição (idade, roupa, altura)
│  ├─ audio.py                  # detecção de frases/ameaças, classificação (sequestro/abuso)
│  ├─ estimator.py              # dead-reckoning e estimador de posição
│  ├─ buffer.py                 # PersistentQueue criptografada (arquivos .buf)
│  ├─ comms.py                  # implementação de canais (internet/sms/sat/whatsapp)
│  ├─ forensics.py              # criação de pacote forense (ZIP + Fernet), hashes
│  ├─ policy_engine.py          # regras: quando ligar 190, quando notificar advogado/psicólogo
│  ├─ profiles.py               # perfis dos contatos de emergência (família, advogados)
│  ├─ pdf_report.py             # gerar PDF detalhado do caso para polícia/familiares
│  ├─ geom_utils.py             # helpers lat/lon, zoom, detectar se é rua/sítio
│  ├─ notifier_workers.py       # workflows assíncronos para enviar mensagens discretas
│  └─ main.py                   # integração, scheduler, CLI para testes
├─ templates/
│  ├─ sms_short.txt
│  ├─ whatsapp_alert.json
│  └─ pdf_template.html
└─ docs/
   ├─ legal_notes.md
   └─ deployment.mddef receber_alerta(tipo_alerta):
    if tipo_alerta in ["Blue Power", "Ultra Red Power"]:
        gerar_imagem_IA(tipo_alerta)
        enviar_alerta(tipo_alerta)

def gerar_imagem_IA(tipo_alerta):
    # Escolhe figurinha / GIF correto
    if tipo_alerta == "Blue Power":
        imagem = "anjinho_azul.gif"
    else:
        imagem = "anjinho_vermelho.gif"
    return imagem

def enviar_alerta(tipo_alerta):
    imagem = gerar_imagem_IA(tipo_alerta)
    mensagem = f"Alerta {tipo_alerta} acionado! — Anjinho da Guarda"
    numero_efemero = gerar_numero_efemero()  # gera número temporário
    enviar_whatsapp(mensagem, imagem, numero_efemero)
    enviar_SMS_backup(mensagem, numero_efemero)

def encerrar_caso(case_id):
    # Mensagem ética
    mensagem_final = "Pela ética, peço que a vítima não saiba que fui eu que passei as informações. — Anjinho da Guarda"
    numero_efemero = gerar_numero_efemero()
    enviar_whatsapp(mensagem_final, None, numero_efemero)
    
    # Limpeza segura
    apagar_mensagens_whatsapp(numero_efemero)
    apagar_mensagens_SMS(numero_efemero)
    limpar_dados_temporarios(case_id)

# Funções auxiliares
def gerar_numero_efemero():
    # Retorna um número temporário/virtual que expira depois do caso
    return "numero_temp_1234"

def apagar_mensagens_whatsapp(numero):
    # Aqui você implementa a lógica para deletar mensagens via API
    print(f"Mensagens do WhatsApp do número {numero} apagadas.")

def apagar_mensagens_SMS(numero):
    # Aqui você implementa a lógica para deletar mensagens SMS via API
    print(f"Mensagens SMS do número {numero} apagadas.")# anjodaguarda.py
"""
Protótipo: AnjoDaGuarda (simulação)
- Offline-first, buffer persistente, dead-reckoning simples, múltiplos canais com backoff.
- Pontos a substituir por produção: integração com GPS/IMU reais, keystore / Fernet key em produção,
  endpoints reais de envio, handlers de erro mais robustos, logs estruturados.
"""

import os
import json
import time
import random
import hashlib
import zipfile
from datetime import datetime, timezone
from collections import deque

# Dependências opcionais
try:
    from cryptography.fernet import Fernet
    CRYPTO_AVAILABLE = True
except Exception:
    CRYPTO_AVAILABLE = False

# ---------- Config ----------
LOCAL_STORAGE = "./anjodaguarda_buffer/"
os.makedirs(LOCAL_STORAGE, exist_ok=True)

# Preferir obter chave segura via variável de ambiente / keystore
FERNET_KEY_ENV = os.environ.get("FERNET_KEY", None)
if FERNET_KEY_ENV and CRYPTO_AVAILABLE:
    FERNET = Fernet(FERNET_KEY_ENV.encode() if isinstance(FERNET_KEY_ENV, str) else FERNET_KEY_ENV)
else:
    FERNET = None  # fallback para pseudo criptografia (NÃO USAR em produção)

# Simulação de disponibilidade de canais
network_state = {
    "internet": False,
    "gsm": False,
    "satellite": False,
    "lora": False,
    "bluetooth_mesh": False
}

# ---------- Utilitários ----------
def now_str():
    return datetime.now(timezone.utc).isoformat()

def sha256_bytes(b: bytes) -> str:
    return hashlib.sha256(b).hexdigest()

def payload_summary(p: dict):
    return {
        "tipo": p.get("tipo"),
        "ts": p.get("ts"),
        "pos": p.get("pos"),
        "submerso": p.get("submerso", False)
    }

def safe_encrypt_bytes(raw: bytes) -> bytes:
    """Usa Fernet se disponível; caso contrário, usa um pseudo método (para simulação)."""
    if FERNET:
        return FERNET.encrypt(raw)
    # simulação (NÃO SEGURO)
    return b"ENC:" + raw[::-1]

def safe_decrypt_bytes(enc: bytes) -> bytes:
    if FERNET:
        return FERNET.decrypt(enc)
    if enc.startswith(b"ENC:"):
        return enc[4:][::-1]
    return enc

# ---------- Buffer persistente ----------
class PersistentQueue:
    def __init__(self, folder=LOCAL_STORAGE):
        self.folder = folder
        self.queue = deque()
        self._load_from_disk()

    def _load_from_disk(self):
        for fname in sorted(os.listdir(self.folder)):
            if fname.endswith(".buf"):
                path = os.path.join(self.folder, fname)
                try:
                    with open(path, "rb") as f:
                        enc = f.read()
                    raw = safe_decrypt_bytes(enc)
                    item = json.loads(raw.decode("utf-8"))
                    self.queue.append((item, path))
                except Exception:
                    # arquivo corrompido ou chave errada -> ignorar
                    continue

    def push(self, item: dict):
        item["_meta"] = {"created_at": now_str()}
        raw = json.dumps(item, ensure_ascii=False).encode("utf-8")
        enc = safe_encrypt_bytes(raw)
        ts = int(time.time()*1000)
        fname = f"item_{ts}_{random.randint(0,9999)}.buf"
        path = os.path.join(self.folder, fname)
        with open(path, "wb") as f:
            f.write(enc)
        self.queue.append((item, path))
        return path

    def pop(self):
        if not self.queue:
            return None
        item, path = self.queue.popleft()
        try:
            if os.path.exists(path):
                os.remove(path)
        except Exception:
            pass
        return item

    def peek_all(self):
        return [it for it, _ in list(self.queue)]

buffer = PersistentQueue()

# ---------- Sensores simulados ----------
def get_gps():
    """Retorna dict (lat, lon, accuracy) ou None"""
    if random.random() < 0.6:
        return {"lat": -6.8 + random.uniform(-0.002,0.002),
                "lon": -38.1 + random.uniform(-0.002,0.002),
                "accuracy_m": 5 + random.random()*10}
    return None

def get_imu_delta():
    return {"dx_m": random.uniform(-1,1), "dy_m": random.uniform(-1,1)}

def sensor_submersed():
    return random.random() < 0.05

# ---------- Estimador de posição (dead-reckoning simples) ----------
class PositionEstimator:
    def __init__(self):
        self.estimate = None

    def update_with_gps(self, gps):
        if gps:
            self.estimate = (gps["lat"], gps["lon"])
            return self.estimate
        if self.estimate:
            delta = get_imu_delta()
            lat, lon = self.estimate
            lat += delta["dx_m"] * 1e-5
            lon += delta["dy_m"] * 1e-5
            self.estimate = (lat, lon)
            return self.estimate
        return None

estimator = PositionEstimator()

# ---------- Stubs de envio (substituir por implementação real) ----------
def send_via_internet(payload: dict):
    if not network_state["internet"]:
        raise ConnectionError("Internet indisponível")
    print("[ENVIANDO] internet:", payload_summary(payload))
    return True

def send_via_gsm_sms(payload: dict):
    if not network_state["gsm"]:
        raise ConnectionError("GSM indisponível")
    print("[ENVIANDO] gsm/sms:", payload_summary(payload))
    return True

def send_via_satellite(payload: dict):
    if not network_state["satellite"]:
        raise ConnectionError("satélite indisponível")
    print("[ENVIANDO] satélite:", payload_summary(payload))
    return True

def send_via_lora(payload: dict):
    if not network_state["lora"]:
        raise ConnectionError("lora indisponível")
    print("[ENVIANDO] lora:", payload_summary(payload))
    return True

def send_via_mesh(payload: dict):
    if not network_state["bluetooth_mesh"]:
        raise ConnectionError("mesh indisponível")
    print("[ENVIANDO] mesh:", payload_summary(payload))
    return True

SENDERS = [send_via_internet, send_via_gsm_sms, send_via_satellite, send_via_lora, send_via_mesh]

# ---------- Forensics / pacote ----------
def create_forensic_package(event: dict, attachments: list, output_path="./package.zip"):
    tmpdir = "./tmp_pkg"
    os.makedirs(tmpdir, exist_ok=True)
    ev_json_path = os.path.join(tmpdir, "event.json")
    with open(ev_json_path, "w", encoding="utf-8") as f:
        json.dump(event, f, ensure_ascii=False, indent=2)
    attached_files = []
    for a in attachments:
        src = a.get("path")
        if src and os.path.exists(src):
            basename = os.path.basename(src)
            dst = os.path.join(tmpdir, basename)
            with open(src, "rb") as fr, open(dst, "wb") as fw:
                fw.write(fr.read())
            attached_files.append(dst)
    zipf = zipfile.ZipFile(output_path, 'w', zipfile.ZIP_DEFLATED)
    zipf.write(ev_json_path, arcname="event.json")
    for fpath in attached_files:
        zipf.write(fpath, arcname=os.path.basename(fpath))
    zipf.close()
    # criptografar zip
    with open(output_path, "rb") as fin:
        data = fin.read()
    enc = safe_encrypt_bytes(data)
    enc_path = output_path + ".enc"
    with open(enc_path, "wb") as fout:
        fout.write(enc)
    # limpar tmpdir se quiser (opcional)
    return enc_path

# ---------- Core: captura de evento e tentativa de envio com retries ----------
def capturar_evento(tipo: str, extra: dict):
    gps = get_gps()
    pos = estimator.update_with_gps(gps)
    submerso = sensor_submersed()
    evento = {
        "tipo": tipo,
        "ts": now_str(),
        "pos": {"lat": pos[0], "lon": pos[1]} if pos else None,
        "gps_raw": gps,
        "submerso": submerso,
        "extra": extra
    }
    path = buffer.push(evento)
    print(f"[CAPTURADO] evento armazenado em buffer: {path} (submerso={submerso})")
    tentar_envio_retries()
    return evento

def tentar_envio_retries(max_attempts_per_item=3):
    # snapshot para evitar mutação enquanto itera
    items = buffer.peek_all()
    for item in items:
        attempts = 0
        sent = False
        while attempts < max_attempts_per_item and not sent:
            for sender in SENDERS:
                try:
                    sender(item)
                    sent = True
                    break
                except ConnectionError:
                    continue
            attempts += 1
            if not sent:
                backoff = 2 ** attempts
                print(f"[RETRY] envio falhou, aguardando {backoff}s")
                time.sleep(backoff)
        if sent:
            buffer.pop()
            print("[OK] Item enviado e removido do buffer")
        else:
            print("[NOK] Item permanece no buffer para retry futuro")

# ---------- Rede monitor (simulação) ----------
def network_monitor_simulation(timesteps=20):
    for t in range(timesteps):
        network_state["internet"] = random.random() < 0.3
        network_state["gsm"] = random.random() < 0.25
        network_state["satellite"] = random.random() < 0.05
        network_state["lora"] = random.random() < 0.15
        network_state["bluetooth_mesh"] = random.random() < 0.2
        print(f"\n[TIME] passo {t} - rede: {network_state}")
        tentar_envio_retries()
        time.sleep(1)

# ---------- Resumo curto (SMS / sat) ----------
def prepare_short_report(event: dict) -> str:
    latlon = event.get("pos")
    ts = event.get("ts")
    typ = event.get("tipo")
    pkg_hash = event.get("_pkg_hash", "nohash")
    short = f"TIPO:{typ};TS:{ts};POS:{latlon};H:{pkg_hash[:12]};ID:{event.get('event_id','noid')}"
    return short

# ---------- Execução de exemplo ----------
if __name__ == "__main__":
    print("=== AnjoDaGuarda (simulação) ===")
    capturar_evento("sequestro_ou_ameaca_crianca", {"descricao": "Homem com jaqueta preta, levou a mão no bolso"})
    time.sleep(0.5)
    capturar_evento("violencia_domestica", {"descricao": "Gritos e objeto quebrando"})
    time.sleep(0.5)
    capturar_evento("intolerancia_religiosa", {"descricao": "Ameaças e símbolos ofensivos"})
    network_monitor_simulation(timesteps=15) Borders](https://aka.ms/PowerToysOverview_MouseWithoutBorders) | [<img src="doc/images/icons/NewPlus.png" alt="New+ icon" height="16"> New+](https://aka.ms/PowerToysOverview_NewPlus) |
| [<img src="doc/images/icons/Peek.png" alt="Peek icon" height="16"> Peek](https://aka.ms/PowerToysOverview_Peek) | [<img src="doc/images/icons/PowerRename.png" alt="PowerRename icon" height="16"> PowerRename](https://aka.ms/PowerToysOverview_PowerRename) | [<img src="doc/images/icons/PowerToys%20Run.png" alt="PowerToys Run icon" height="16"> PowerToys Run](https://aka.ms/PowerToysOverview_PowerToysRun) |
| [<img src="doc/images/icons/PowerAccent.png" alt="Quick Accent icon" height="16"> Quick Accent](https://aka.ms/PowerToysOverview_QuickAccent) | [<img src="doc/images/icons/Registry%20Preview.png" alt="Registry Preview icon" height="16"> Registry Preview](https://aka.ms/PowerToysOverview_RegistryPreview) | [<img src="doc/images/icons/MeasureTool.png" alt="Screen Ruler icon" height="16"> Screen Ruler](https://aka.ms/PowerToysOverview_ScreenRuler) |
| [<img src="doc/images/icons/Shortcut%20Guide.png" alt="Shortcut Guide icon" height="16"> Shortcut Guide](https://aka.ms/PowerToysOverview_ShortcutGuide) | [<img src="doc/images/icons/PowerOCR.png" alt="Text Extractor icon" height="16"> Text Extractor](https://aka.ms/PowerToysOverview_TextExtractor) | [<img src="doc/images/icons/Workspaces.png" alt="Workspaces icon" height="16"> Workspaces](https://aka.ms/PowerToysOverview_Workspaces) |
| [<img src="doc/images/icons/ZoomIt.png" alt="ZoomIt icon" height="16"> ZoomIt](https://aka.ms/PowerToysOverview_ZoomIt) |   |   |


## 📋 Installation

For detailed installation instructions, visit the [installation docs](https://learn.microsoft.com/windows/powertoys/install). 

Before you begin, make sure your device meets the system requirements:

> [!NOTE]
> - Windows 11 or Windows 10 version 2004 (20H1 / build 19041) or newer
> - 64-bit processor: x64 or ARM64
> - Latest stable version of [Microsoft Edge WebView2 Runtime](https://go.microsoft.com/fwlink/p/?LinkId=2124703) is installed via the bootstrapper during setup

Choose one of the installation methods below:

<details>
<summary>Download .exe from GitHub</summary>

Go to the [PowerToys GitHub releases][github-release-link], click Assets to reveal the downloads, and choose the installer that matches your architecture and install scope. For most devices, that's the x64 per-user installer.

<!-- items that need to be updated release to release -->
[github-next-release-work]: https://github.com/microsoft/PowerToys/issues?q=is%3Aissue+milestone%3A%22PowerToys+0.96%22
[github-current-release-work]: https://github.com/microsoft/PowerToys/issues?q=is%3Aissue+milestone%3A%22PowerToys+0.95%22
[ptUserX64]: https://github.com/microsoft/PowerToys/releases/download/v0.95.0/PowerToysUserSetup-0.95.0-x64.exe 
[ptUserArm64]: https://github.com/microsoft/PowerToys/releases/download/v0.95.0/PowerToysUserSetup-0.95.0-arm64.exe 
[ptMachineX64]: https://github.com/microsoft/PowerToys/releases/download/v0.95.0/PowerToysSetup-0.95.0-x64.exe 
[ptMachineArm64]: https://github.com/microsoft/PowerToys/releases/download/v0.95.0/PowerToysSetup-0.95.0-arm64.exe
 
|  Description   | Filename |
|----------------|----------|
| Per user - x64       | [PowerToysUserSetup-0.95.0-x64.exe][ptUserX64] |
| Per user - ARM64     | [PowerToysUserSetup-0.95.0-arm64.exe][ptUserArm64] |
| Machine wide - x64   | [PowerToysSetup-0.95.0-x64.exe][ptMachineX64] |
| Machine wide - ARM64 | [PowerToysSetup-0.95.0-arm64.exe][ptMachineArm64] |

</details>

<details>
<summary>Microsoft Store</summary>
You can easily install PowerToys from the Microsoft Store:
<p>
  <a style="text-decoration:none" href="https://aka.ms/getPowertoys">
    <picture>
      <source media="(prefers-color-scheme: light)" srcset="doc/images/readme/StoreBadge-dark.png" width="148" />
      <img src="doc/images/readme/StoreBadge-light.png" width="148" />
  </picture></a>
</p>
</details>


<details>
<summary>WinGet</summary>

Download PowerToys from [WinGet][winget-link]. Updating PowerToys via winget will respect the current PowerToys installation scope. To install PowerToys, run the following command from the command line / PowerShell:

*User scope installer [default]*
```powershell
winget install Microsoft.PowerToys -s winget
```

*Machine-wide scope installer*
```powershell
winget install --scope machine Microsoft.PowerToys -s winget
```
</details>

<details>
<summary>Other methods</summary>

There are [community driven install methods](./doc/unofficialInstallMethods.md) such as Chocolatey and Scoop. If these are your preferred install solutions, you can find the install instructions there.
</details>

## ✨ What's new
**Version 0.95 (October 2025)**

For an in-depth look at the latest changes, visit the [Windows Command Line blog](https://aka.ms/powertoys-releaseblog).

**✨ Highlights**
 - **NEW:** The **Light Switch** utility in PowerToys allows you to automatically switch between light and dark themes in Windows based on the time of day.
 - Command Palette delivered major search performance gains (new fuzzy matcher and smarter fallbacks) improving relevance and speed.
 - Peek can now be activated using just the Spacebar!
 - Find My Mouse added transparent spotlight with independent backdrop opacity, boosting focus and accessibility.
 - Settings now lets you delete shortcuts entirely and ignore conflicts.
 - Mouse Pointer Crosshairs gained orientation options (vertical / horizontal / both) for customizable accessibility. Thanks [@mikehall-ms](https://github.com/mikehall-ms)!
 - PowerRename fixed enumeration counter skipping ensuring reliable batch renames. Thanks [@daverayment](https://github.com/daverayment)!
 - ZoomIt restored legacy draw and snipping behaviors, and fixed recording issues, improving reliability. Thanks [@chakrik73](https://github.com/chakrik73)!

### Command Palette
 - Applied conditional margin for icon-only tags to tighten layout. Thanks [@samrueby](https://github.com/samrueby)
 - Improved the reliability of accessing Command Palette settings through PowerToys Settings and executing other x-cmdpal:// protocol commands. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Enabled AOT by default for improved performance while simplifying publish configs.
 - Replaced service state color dots with play/pause/stop icons for enhanced accessibility. Thanks [@samrueby](https://github.com/samrueby)
 - Fixed filter dropdown sync and crash by binding SelectedValue and raising UI-thread notifications. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Ensured long links wrap correctly in details view.
 - Removed animation and enforced minimum width on filter dropdown for clarity. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Restored focus to More button after ESC closes context menu, improving keyboard flow. Thanks [@chatasweetie](https://github.com/chatasweetie)
 - Marked main and toast windows as tool windows to keep them out of Alt+Tab while preserving style. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Fixed AOT template and theming issues for filter separators. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Introduced grid layouts (small, medium, gallery) for richer page presentation.
 - Materialized result lists to avoid rescoring overhead.
 - Disabled problematic selection TextToSuggest behind environment flag.
 - Major search performance improvements (new fuzzy matcher, smarter fallbacks, fewer exceptions).
 - Added context menu "Show Details" command when details pane is hidden.
 - Reduced window flicker by avoiding unnecessary cloaking. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Restored EmptyContent rendering for blank states. Thanks [@DevLGuilherme](https://github.com/DevLGuilherme)
 - Saved new state even if prior app state file was corrupt (better resilience). Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Migrated settings window to WinUI TitleBar control. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Prevented crash on duplicate keybindings and simplified matching. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Hotkeys now always respect the “Ignore shortcut in fullscreen” setting. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Hid search box on content pages, improving focus and accessibility, and added Home title. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Blocked Ctrl+I from inserting stray tabs in search box.
 - Logged HRESULT codes in error logs for deeper diagnostics. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Advanced font and emoji icon classification and alignment improvements. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Ensured that fallback command icons are visible on the extension settings page. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Fixed breadcrumb margin misalignment (visual polish). Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Truncated overly long command labels with ellipsis to prevent overflow.
 - Added a setting to configure the page transition animation.
 - Collection of small improvements and nits for Run Commands.
 - Improved bookmarks performance and experience. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Added Ctrl+O shortcut in Clipboard History to open links directly.
 - Resolved conflict with external software that blocked Command Palette from hiding.
 - Updated context menu items to reflect name and icon changes, and ensured application icons are displayed correctly. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Added Alt+Home shortcut to return immediately to the Command Palette home page. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Fixed a crash when displaying code blocks in markdown on detail or content pages. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Fixed an issue where the search bar icon and title were not updated when rapidly switching pages. Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Improved the appearance of the search box in the context menu.


### Command Palette Extensions
 - Replaced localized WebSearch setting keys with stable literals and numeric history count. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Enabled advanced markdown tables and emphasis extensions. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Added setting to choose Clipboard History primary action (Paste vs Copy). Thanks [@jiripolasek](https://github.com/jiripolasek)
 - Added actionable empty-state hints for File Search (search PC / open indexing settings). Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Ensured all WinGet extension assets copy reliably to output. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Improved Run command line parsing for paths with spaces; sped up related tests.
 - Updated WebSearch extension icon set for enhanced clarity and contrast. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Added Terminal profile sort order setting including MRU tracking. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Added Uninstall Application command (UWP direct, Win32 via Settings). Thanks [@mKpwnz](https://github.com/mKpwnz)!
 - Deferred WinGet details loading and added timing logs.
 - Removed LINQ from All Apps extension for performance.
 - Added standardized key chord system + shortcuts to File Search commands. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Added Terminal channel filter & remembered selection option. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Enabled loading local/data/app images in markdown with sizing hints. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Added external extension reload via x-cmdpal://reload (configurable). Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Instant WebSearch history updates with in-memory store & events. Thanks [@jiripolasek](https://github.com/jiripolasek)!
 - Added keep-after-paste option and safe delete with confirmation for Clipboard History. Thanks [@jiripolasek](https://github.com/jiripolasek)!

### Environment Variables
 - Replaced custom window chrome with WinUI TitleBar for cleaner, maintainable Environment Variables UI.

### File Locksmith
 - Adopted WinUI TitleBar to simplify window chrome while preserving appearance.

### Find My Mouse
 - Added transparent spotlight support with separate backdrop opacity; migrated to Windows App SDK composition APIs.

### Hosts File Editor
 - Migrated to native WinUI TitleBar for cleaner, maintainable window chrome.

### Light Switch
 - Introduced as a brand-new PowerToy module.
 - Automatically switches between light and dark themes.
 - Supports time-based scheduling or location-based sunrise/sunset switching.
 - Supports using a keyboard shortcut to force a change.
 - Supports filtering changes for Apps and/or System Theme.

### Mouse Pointer Crosshairs
 - Added Esc key to cancel active gliding cursor sequence. Thanks [@mikehall-ms](https://github.com/mikehall-ms)!
 - Added orientation option (vertical / horizontal / both) for crosshairs customization. Thanks [@mikehall-ms](https://github.com/mikehall-ms)!

### Mouse Without Borders
 - Continued Common class refactor (part 5/7) by extracting clipboard and init/cleanup logic into focused classes. Thanks [@mikeclayton](https://github.com/mikeclayton)!

 - Fix connection failures caused by conflicting MachineId across machines.  Thanks [@noraa-junker](https://github.com/noraa-junker) for troubleshooting!

### Peek
 - Added the option to activate Peek with just the Spacebar.

### PowerRename
 - Fixed enumeration counter skipping when regex replacement equals original filename (counters now advance reliably). Thanks [@daverayment](https://github.com/daverayment)!

### Quick Accent
 - Expanded Welsh layout with acute, grave, and dieresis variants for vowels (consistent ordering). Thanks [@PesBandi](https://github.com/PesBandi)!

### Registry Preview
 - Migrated to native TitleBar and AppWindow APIs for cleaner window chrome.

### Screen Ruler
 - Fixed ARM64 crash by aligning cursor position structure to 8-byte boundary.

### Settings
 - Added ability to ignore specific hotkey conflicts to reduce noise.
 - Stopped creating backup directory during dry-run status checks (cleaner first-run). 
 - Standardized casing and localization for ZoomIt and modules header.
 - Improved search results page accessibility and conditional module grouping.

### ZoomIt
 - Updated resource file to reflect standalone v9.01 and current copyright year. Thanks [@foxmsft](https://github.com/foxmsft)!
 - Restored legacy draw/snipping behaviors and fixed recording race conditions. Thanks [@chakrik73](https://github.com/chakrik73)!
 - Added smooth image option for improved zoom quality using GDI+ for static zoom and Magnifier API for live zoom. Thanks [@markrussinovich](https://github.com/markrussinovich)!

 ### Documentation
 - New Microsoft Learn documentation for the Light Switch module.
 - New dev docs for the Light Switch module.

### Development (Area-Build & Area-Tests) 
- Allowed debug launches to continue when modules fail to load, speeding developer iteration. 
- Fixed spell checker dictionary entry (advapi) to eliminate false error. 
- Added VS Code development guide and launch configs to streamline cross-editor workflows. 
- Upgraded Windows App SDK and related dependencies to 1.8 for newer platform features. 
- Rewrote YAML comment to resolve new spell checker forbidden pattern. Thanks [@jiripolasek](https://github.com/jiripolasek)! 
- Corrected solution structure by returning misplaced Common project, reducing build confusion. 
- Modernized build scripts with shared helpers and VS environment autodetection for simpler CLI builds. 
- Standardized build scripts and platform detection to improve reliability and reuse. 
- Added missing Command Palette version bump to align module release cadence. 
- Added EXECUTEDEFAULT term to dictionary to prevent regression build failures. Thanks [@jiripolasek](https://github.com/jiripolasek)! 
- Introduced nightly pre-warm pipeline and configurable MSBuild cache mode to improve CI performance. 
- Resolved CI forbidden pattern spelling complaint to keep pipelines green. 
- Added AI contributor instruction set to clarify code area expectations. 
- Added accessibility IDs to settings and FancyZones toggles, stabilizing UI tests. 
- Added automatic log collection on UI test failures to speed root cause analysis. 
- Stabilized Mouse Utils tests by switching to AccessibilityId selectors. 
- Added Screen Ruler UI test coverage to validate core measurement workflows. 

## 🛣️ Roadmap 
We are planning some nice new features and improvements for the next releases – a revamped Keyboard Manager UI, custom endpoint and local model support for Advanced Paste, Command Palette improvements and a brand-new Shortcut Guide experience! Stay tuned for [v0.96][github-next-release-work]!

## ❤️ PowerToys Community 
The PowerToys team is extremely grateful to have the [support of an amazing active community][community-link]. The work you do is incredibly important. PowerToys wouldn't be nearly what it is today without your help filing bugs, updating documentation, guiding the design, or writing features. We want to say thank you and take time to recognize your work. Your contributions and feedback improve PowerToys month after month!

## Contributing 
This project welcomes contributions of all types. Besides coding features / bug fixes, other ways to assist include spec writing, design, documentation, and finding bugs. We are excited to work with the power user community to build a set of tools for helping you get the most out of Windows. We ask that **before you start work on a feature that you would like to contribute**, please read our [Contributor's Guide](CONTRIBUTING.md). We would be happy to work with you to figure out the best approach, provide guidance and mentorship throughout feature development, and help avoid any wasted or duplicate effort. Most contributions require you to agree to a [Contributor License Agreement (CLA)][oss-CLA] declaring that you grant us the rights to use your contribution and that you have permission to do so. For guidance on developing for PowerToys, please read the [developer docs](./doc/devdocs) for a detailed breakdown. This includes how to setup your computer to compile. 

## Code of Conduct 
This project has adopted the [Microsoft Open Source Code of Conduct][oss-conduct-code]. 

## Privacy Statement 
The application logs basic diagnostic data (telemetry). For more privacy information and what we collect, see our [PowerToys Data and Privacy documentation](https://aka.ms/powertoys-data-and-privacy-documentation). 

[oss-CLA]: https://cla.opensource.microsoft.com 
[oss-conduct-code]: CODE_OF_CONDUCT.md 
[community-link]: COMMUNITY.md 
[github-release-link]: https://aka.ms/installPowerToys 
[microsoft-store-link]: https://aka.ms/getPowertoys 
[winget-link]: https://github.com/microsoft/winget-cli#installing-the-client 
[roadmap]: https://github.com/microsoft/PowerToys/wiki/Roadmap
[privacy-link]: http://go.microsoft.com/fwlink/?LinkId=521839 
[loc-bug]: https://github.com/microsoft/PowerToys/issues/new?assignees=&labels=&template=translation_issue.md&title= 
[usingPowerToys-docs-link]: https://aka.ms/powertoys-docs