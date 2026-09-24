# AGENTS.md (Türkçe)

🌐 **Languages:** 🇺🇸 [English](../../../AGENTS.md) · 🇸🇦 [ar](../ar/AGENTS.md) · 🇦🇿 [az](../az/AGENTS.md) · 🇧🇬 [bg](../bg/AGENTS.md) · 🇧🇩 [bn](../bn/AGENTS.md) · 🇨🇿 [cs](../cs/AGENTS.md) · 🇩🇰 [da](../da/AGENTS.md) · 🇩🇪 [de](../de/AGENTS.md) · 🇪🇸 [es](../es/AGENTS.md) · 🇮🇷 [fa](../fa/AGENTS.md) · 🇫🇮 [fi](../fi/AGENTS.md) · 🇫🇷 [fr](../fr/AGENTS.md) · 🇮🇳 [gu](../gu/AGENTS.md) · 🇮🇱 [he](../he/AGENTS.md) · 🇮🇳 [hi](../hi/AGENTS.md) · 🇭🇺 [hu](../hu/AGENTS.md) · 🇮🇩 [id](../id/AGENTS.md) · 🇮🇹 [it](../it/AGENTS.md) · 🇯🇵 [ja](../ja/AGENTS.md) · 🇰🇷 [ko](../ko/AGENTS.md) · 🇮🇳 [mr](../mr/AGENTS.md) · 🇲🇾 [ms](../ms/AGENTS.md) · 🇳🇱 [nl](../nl/AGENTS.md) · 🇳🇴 [no](../no/AGENTS.md) · 🇵🇭 [phi](../phi/AGENTS.md) · 🇵🇱 [pl](../pl/AGENTS.md) · 🇵🇹 [pt](../pt/AGENTS.md) · 🇧🇷 [pt-BR](../pt-BR/AGENTS.md) · 🇷🇴 [ro](../ro/AGENTS.md) · 🇷🇺 [ru](../ru/AGENTS.md) · 🇸🇰 [sk](../sk/AGENTS.md) · 🇸🇪 [sv](../sv/AGENTS.md) · 🇰🇪 [sw](../sw/AGENTS.md) · 🇮🇳 [ta](../ta/AGENTS.md) · 🇮🇳 [te](../te/AGENTS.md) · 🇹🇭 [th](../th/AGENTS.md) · 🇺🇦 [uk-UA](../uk-UA/AGENTS.md) · 🇵🇰 [ur](../ur/AGENTS.md) · 🇻🇳 [vi](../vi/AGENTS.md) · 🇨🇳 [zh-CN](../zh-CN/AGENTS.md) · 🇹🇼 [zh-TW](../zh-TW/AGENTS.md)

---

@AGENTS.md

**Tüm proje kuralları [`AGENTS.md`](AGENTS.md) dosyasında yer almaktadır** — her yapay zeka asistanı için tek doğruluk kaynağıdır (mimari, kurallar, testler, kalite kapıları, git iş akışı, 23 Katı Kural, PII öğrenimleri). Tamamını okuyun; buraya yeniden proje kuralları eklemeyin. Aşağıdaki her şey YALNIZCA AI coding agents için geçerlidir — `AGENTS.md` içinde zaten tanımlanmış kuralların operasyonel ayrıntılarıdır.

## Worktree İzolasyonu — AI coding agents Özel Notları

Tam zorunlu worktree protokolü (hedef dal onayı, `.worktrees/` kurallı yolu, `cp -al` node_modules, kaldırma kuralları) `AGENTS.md` → Git Workflow → "Worktree isolation" bölümündedir. Taşınabilir agent noktaları:

- Operatör daha önce belirtmediyse, hedef dalı `AskUserQuestion` (Katı Kural #19) ile onaylayın.
- Yerel `EnterWorktree` aracını tercih edin — worktree'leri zaten `.worktrees/` altında oluşturur (kurallı yol). Belgelenen `git worktree add` komutuyla worktree oluşturun, ardından `path` parametresi ile `EnterWorktree` çağırın.

## Oturumlar Arası Güvenlik — Agent Notları

Katı Kurallar #19/#21/#22 (`AGENTS.md` içinde) paralel oturumları yönetir. Bu ortam için operasyonel hatırlatmalar:

- **Git'e dokunan her alt ajanın isteminde `git stash` yasağını kelimesi kelimesine tekrarlayın** (Agent tool / Workflow betikleri) — alt ajanlar bu dosyayı devralmaz ve kaydedilen stash olayı bir alt ajan aracılığıyla gerçekleşti.
- _Bu oturumda_ oluşturmadığınız herhangi bir PR'ı birleştirmeden veya push etmeden önce `git worktree list` çalıştırın ve `gh pr view <N> --json state,headRefOid` kontrolü yapın (Katı Kural #22b).
- Her oturumu, ana checkout başladığı dalda olacak şekilde sonlandırın.

## Superpowers / Planlama Yapıtları — Yol Geçersiz Kılmaları

`_tasks/` kuralı `AGENTS.md` → "Planning & Research Artifacts" içinde tanımlanmıştır. Superpowers yetenekleri `docs/…` dizinini işaret eden varsayılanlarla gelir — bu varsayılanlar **burada geçersiz kılınmıştır**. Bir superpowers yeteneği "saved to `docs/superpowers/plans/…`" gibi bir yol duyurduğunda, yazmadan önce onu `_tasks/…` eşdeğerine yeniden yazın:

| Yapıt (Yetenek)                         | Varsayılan (KULLANMAYIN)  | Bunun yerine buraya kaydedin                                  |
| --------------------------------------- | ------------------------- | ------------------------------------------------------------- |
| Planlar (`writing-plans`)               | `docs/superpowers/plans/` | `_tasks/superpowers/plans/YYYY-MM-DD-<feature>.md`            |
| Şartnameler / tasarım (`brainstorming`) | `docs/superpowers/specs/` | `_tasks/superpowers/specs/YYYY-MM-DD-<topic>-design.md`       |
| Araştırma (`deep-research`, ad-hoc)     | `docs/research/`          | `_tasks/research/…`                                           |
| Devirler (`/handoff`)                   | —                         | `_tasks/hands-off/<YYYY-MM-DD>_<branch>_v<versão>_sess-<id>/` |

Bu yapıtları `_tasks/` deposu içinde commit edin (`git -C _tasks …`), asla ana depoda değil.

## Geçici Dosyalar — `/tmp` Değil `_artifacts/` Kullanın

Bu proje, çalışma ortamının varsayılan oturum karalama alanını (`harness-specific scratch directories`) geçersiz kılar. Geçici/çalışma dosyalarını — dışa aktarmaları, oluşturulan zip'leri, tek seferlik ara çıktıları, aksi halde `/tmp` içine koyacağınız her şeyi — bunun yerine `/home/diegosouzapw/dev/proxys/OmniRoute/_artifacts/` dizinine yazın.

- `_artifacts/` bir kök `_*` yoludur: zaten gitignore edilmiştir (`AGENTS.md` → "Root `_*` paths"), yalnızca diskte yaşar, asla takip edilmez.
- Gerekçe: karalama çıktılarını proje içinde tutmak (vs `/tmp`), operatörün geçici her şeyi tek bir yerde bulup silmesini kolaylaştırır.
- Bunu `_tasks/` (Katı Kural #23, kalıcı planlar/şartnameler/araştırmalar için kendi özel git deposu) ile **karıştırmayın** — `_artifacts/` yalnızca tek kullanımlık çalışma dosyaları içindir.

## PR Açmadan Önce Base-Green Kontrolü

Bir dal açmadan veya PR oluşturmadan önce base-green kontrolünü çalıştırın (`AGENTS.md` → Git Workflow → "Base-green check"; proje yetenekleri bunu `.agents/skills/_shared/base-green.md` olarak referans alır). Temel uç (base tip) kırmızı iken açılan bir PR, gövdesinde `⚠️ base-red inherited: #<issue>` taşımalıdır. Birikmiş kırmızı durumu (temel uç + kırmızı PR'lar) boşaltmak için `/sweep-reds` yeteneğini kullanın.
