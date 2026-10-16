<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover, interactive-widget=resizes-content" />
<meta name="theme-color" content="#000000" />
<title>GrenX Voice Studio</title>
<style>
  *{margin:0;padding:0;box-sizing:border-box;font-family:-apple-system,'Segoe UI',Roboto,sans-serif;-webkit-tap-highlight-color:transparent}
  html,body{height:100%;width:100%;overflow:hidden;overscroll-behavior:none;-webkit-user-select:none;user-select:none;-webkit-touch-callout:none;position:fixed;inset:0}
  body{background:#000;color:#fff;display:flex;justify-content:center;background-image:radial-gradient(circle at 15% 15%,rgba(0,255,180,.08),transparent 45%),radial-gradient(circle at 85% 85%,rgba(120,0,255,.1),transparent 45%)}
  .app{width:100%;max-width:720px;height:100dvh;display:grid;grid-template-rows:auto 1fr auto;position:relative;z-index:1;overflow:hidden}
  @supports not (height:100dvh){.app{height:100vh}}
  .header{display:flex;align-items:center;justify-content:space-between;padding:10px 14px;height:58px;flex-shrink:0;background:rgba(0,0,0,.85);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);border-bottom:1px solid rgba(255,255,255,.06);position:relative;z-index:20}
  .brand{display:flex;align-items:center;gap:10px;min-width:0;flex:1}
  .brand-logo{width:36px;height:36px;border-radius:10px;background:linear-gradient(135deg,#00ffb4,#7c3aed);display:flex;align-items:center;justify-content:center;color:#000;box-shadow:0 4px 14px rgba(0,255,180,.35);flex-shrink:0}
  .brand-logo svg{width:19px;height:19px;stroke:#000;fill:none;stroke-width:2.2;stroke-linecap:round;stroke-linejoin:round}
  .brand-text{min-width:0;flex:1}
  .brand-text h1{font-size:13.5px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .brand-text p{font-size:10px;color:#7d8b99;margin-top:1px;font-family:'SF Mono',monospace;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .header-actions{display:flex;gap:6px;flex-shrink:0}
  .btn-round{width:36px;height:36px;border-radius:50%;background:#1c1c1e;border:none;color:#fff;display:flex;align-items:center;justify-content:center;cursor:pointer;transition:.2s;flex-shrink:0}
  .btn-round:active{transform:scale(.9);background:#2c2c2e}
  .btn-round svg{width:17px;height:17px;stroke:#fff;fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
  .chat{min-height:0;overflow-y:auto;overflow-x:hidden;padding:14px 14px 12px;display:flex;flex-direction:column;gap:12px;-webkit-overflow-scrolling:touch;overscroll-behavior:contain;scroll-behavior:smooth}
  .chat::-webkit-scrollbar{width:0}
  .welcome{flex:1;display:flex;flex-direction:column;justify-content:center;align-items:center;padding:20px;text-align:center;min-height:0}
  .welcome-avatar{width:96px;height:96px;border-radius:50%;background:linear-gradient(135deg,#00ffb4,#7c3aed,#ff5f1f);background-size:200% 200%;display:flex;align-items:center;justify-content:center;font-size:38px;font-weight:900;color:#000;box-shadow:0 12px 40px rgba(0,255,180,.4);margin-bottom:18px;flex-shrink:0;animation:avatarPulse 3s ease-in-out infinite,gradientShift 4s ease infinite;font-family:'SF Mono',monospace}
  @keyframes avatarPulse{0%,100%{transform:scale(1)}50%{transform:scale(1.05)}}
  @keyframes gradientShift{0%,100%{background-position:0% 50%}50%{background-position:100% 50%}}
  .welcome h2{font-size:21px;font-weight:700;margin-bottom:8px;background:linear-gradient(135deg,#fff,#00ffb4,#a855f7);background-size:200% 200%;-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;animation:gradientShift 5s ease infinite;font-family:'SF Mono',monospace}
  .welcome p{font-size:12.5px;color:#8e8e93;line-height:1.5;max-width:340px}
  .welcome-chips{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin-top:18px;max-width:400px}
  .chip{background:rgba(255,255,255,.05);border:1px solid rgba(255,255,255,.1);color:#c5cfdb;padding:8px 14px;border-radius:20px;font-size:12px;cursor:pointer;transition:.2s;display:flex;align-items:center;gap:6px}
  .chip:active{transform:scale(.96);border-color:#00ffb4;color:#00ffb4}
  .msg{max-width:85%;padding:11px 14px;border-radius:18px;font-size:15px;line-height:1.5;word-wrap:break-word;overflow-wrap:anywhere;white-space:pre-wrap;animation:fade .25s cubic-bezier(.2,.8,.2,1);flex-shrink:0}
  @keyframes fade{from{opacity:0;transform:translateY(8px) scale(.97)}to{opacity:1;transform:translateY(0) scale(1)}}
  .msg.user{align-self:flex-end;background:#2c2c2e;color:#fff;border-bottom-right-radius:6px}
  .msg.bot{align-self:flex-start;background:#141416;color:#e5e5e7;border:1px solid rgba(255,255,255,.06);border-bottom-left-radius:6px;max-width:92%;padding:12px 14px}
  .msg.bot .label{display:flex;align-items:center;gap:7px;font-size:12px;font-weight:600;color:#00ffb4;margin-bottom:8px;font-family:'SF Mono',monospace}
  .msg.bot .label .dot{width:20px;height:20px;border-radius:50%;background:linear-gradient(135deg,#00ffb4,#7c3aed);background-size:200% 200%;display:flex;align-items:center;justify-content:center;font-size:10px;color:#000;font-weight:800;flex-shrink:0;animation:gradientShift 3s ease infinite}
  .msg.bot pre{background:#0d1117;border:1px solid #1f2a36;border-radius:10px;padding:12px;margin:8px 0;overflow-x:auto;font-family:'SF Mono',Consolas,monospace;font-size:12.5px;line-height:1.5;white-space:pre;color:#e6edf3;max-width:100%}
  .msg.bot code{background:#0d1117;padding:2px 6px;border-radius:5px;font-family:'SF Mono',Consolas,monospace;font-size:12.5px;color:#00ffb4}
  .msg.bot pre code{background:none;padding:0;color:inherit}
  .msg.bot video.gen{max-width:100%;border-radius:12px;margin:8px 0;display:block;border:1px solid rgba(255,255,255,.1);cursor:pointer;background:#0a0a0a}
  .msg.error{background:#2a1416;border:1px solid #5f2329;color:#ff8f9a;border-radius:14px;padding:10px 14px;font-size:13.5px;align-self:flex-start;max-width:85%}
  .msg.user img.att{max-width:200px;border-radius:10px;display:block;margin-top:6px;border:1px solid rgba(255,255,255,.1)}
  .img-wrap{position:relative;width:100%;min-height:200px;border-radius:12px;overflow:hidden;background:#0d1117;display:flex;align-items:center;justify-content:center;margin:8px 0;border:1px solid rgba(255,255,255,.08)}
  .img-loader{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:10px;color:#00ffb4;font-size:12px;font-family:'SF Mono',monospace;padding:20px;text-align:center}
  .img-spin{width:36px;height:36px;border:3px solid rgba(0,255,180,.2);border-top-color:#00ffb4;border-radius:50%;animation:spin 1s linear infinite}
  @keyframes spin{to{transform:rotate(360deg)}}
  .img-fallback{display:flex;flex-direction:column;align-items:center;gap:10px;padding:20px;text-align:center}
  .img-fallback .icon{font-size:32px}
  .img-fallback .text{color:#ff8f9a;font-size:13px;line-height:1.4}
  .img-fallback button{background:linear-gradient(135deg,#00ffb4,#00d9ff);color:#000;border:none;padding:8px 16px;border-radius:8px;font-weight:700;font-size:12px;cursor:pointer;font-family:'SF Mono',monospace}
  .img-fallback button:active{transform:scale(.96)}
  .msg.bot img.gen{max-width:100%;border-radius:12px;display:block;border:1px solid rgba(255,255,255,.1);cursor:pointer;background:#0a0a0a;margin:0}
  .edit-steps{display:flex;flex-direction:column;gap:6px;margin-top:8px}
  .edit-step{display:flex;align-items:center;gap:8px;font-size:12.5px;color:#8e8e93;font-family:'SF Mono',monospace;transition:.3s}
  .edit-step.active{color:#00ffb4}
  .edit-step.done{color:#7d8b99}
  .edit-step .ico{width:16px;height:16px;border-radius:50%;border:2px solid #2c2c2e;display:flex;align-items:center;justify-content:center;font-size:9px;flex-shrink:0;transition:.3s}
  .edit-step.active .ico{border-color:#00ffb4;background:rgba(0,255,180,.15)}
  .edit-step.done .ico{border-color:#00ffb4;background:#00ffb4;color:#000}
  .edit-step .ico.spin{border:2px solid rgba(0,255,180,.2);border-top-color:#00ffb4;animation:spin 1s linear infinite}
  .edit-timer{margin-top:6px;font-size:10.5px;color:#7d8b99;font-family:'SF Mono',monospace}
  .speak-btn{display:inline-flex;align-items:center;gap:5px;background:rgba(0,255,180,.08);border:1px solid rgba(0,255,180,.2);color:#00ffb4;font-size:10.5px;font-weight:600;padding:4px 10px;border-radius:8px;cursor:pointer;margin-top:8px;font-family:'SF Mono',monospace;transition:.15s}
  .speak-btn:active{transform:scale(.94);background:rgba(0,255,180,.15)}
  .speak-btn svg{width:11px;height:11px;stroke:currentColor;fill:none;stroke-width:2.4;stroke-linecap:round;stroke-linejoin:round}
  .typing{display:flex;gap:5px;padding:12px 15px;background:#141416;border:1px solid rgba(255,255,255,.06);border-radius:18px;width:fit-content;align-self:flex-start;align-items:center}
  .typing span{width:7px;height:7px;background:linear-gradient(135deg,#00ffb4,#7c3aed);background-size:200% 200%;border-radius:50%;animation:bounce 1.2s ease-in-out infinite,gradientShift 2s ease infinite}
  .typing span:nth-child(2){animation-delay:.12s,.12s}
  .typing span:nth-child(3){animation-delay:.24s,.24s}
  @keyframes bounce{0%,60%,100%{transform:translateY(0) scale(.9);opacity:.4}30%{transform:translateY(-5px) scale(1.1);opacity:1}}
  .input-wrap{flex-shrink:0;padding:8px 14px calc(10px + env(safe-area-inset-bottom,0px));background:rgba(0,0,0,.9);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);border-top:1px solid rgba(255,255,255,.05);position:relative;z-index:3}
  .attach-bar{display:none;margin-bottom:8px;padding:6px;background:rgba(255,255,255,.05);border:1px solid rgba(255,255,255,.1);border-radius:12px;align-items:center;gap:8px}
  .attach-bar.show{display:flex}
  .attach-bar img{width:40px;height:40px;border-radius:8px;object-fit:cover;border:1px solid rgba(255,255,255,.1)}
  .attach-bar .name{flex:1;font-size:11px;color:#c5cfdb;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .attach-bar button{width:26px;height:26px;border-radius:50%;background:rgba(255,255,255,.08);border:none;color:#8e8e93;display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0}
  .attach-bar button svg{width:13px;height:13px;stroke:currentColor;stroke-width:2.4;fill:none;stroke-linecap:round}
  .input-bar{display:flex;align-items:flex-end;gap:4px;background:#1c1c1e;border:1px solid rgba(0,255,180,.15);border-radius:26px;padding:5px 5px 5px 6px;min-height:48px;transition:border-color .2s,box-shadow .2s}
  .input-bar.focus{border-color:#00ffb4;box-shadow:0 0 20px rgba(0,255,180,.2)}
  .ic-btn{width:34px;height:34px;border-radius:50%;background:transparent;border:none;color:#8e8e93;display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0;transition:.2s;align-self:flex-end}
  .ic-btn:active{background:rgba(255,255,255,.1);transform:scale(.9)}
  .ic-btn svg{width:19px;height:19px;stroke:currentColor;fill:none;stroke-width:1.9;stroke-linecap:round;stroke-linejoin:round}
  .ic-btn.mic-active{background:linear-gradient(135deg,#ff2f5f,#ff5f1f);color:#fff;animation:micPulse 1s ease infinite}
  .ic-btn.mic-active svg{stroke:#fff}
  @keyframes micPulse{0%,100%{transform:scale(1)}50%{transform:scale(1.08)}}
  .input-bar textarea{flex:1;background:none;border:none;outline:none;color:#fff;font-size:15px;padding:8px 4px;resize:none;max-height:90px;min-height:22px;line-height:1.4;font-family:inherit;overflow-y:auto;-webkit-appearance:none;align-self:flex-end}
  .input-bar textarea::placeholder{color:#5f6f7d}
  .send-btn{width:36px;height:36px;border-radius:50%;background:linear-gradient(135deg,#00ffb4,#7c3aed);background-size:200% 200%;border:none;color:#000;cursor:pointer;display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:.2s;animation:gradientShift 4s ease infinite;align-self:flex-end}
  .send-btn:active{transform:scale(.88)}
  .send-btn:disabled{opacity:.4;cursor:not-allowed;animation:none;background:#2c2c2e;color:#8e8e93}
  .send-btn svg{width:16px;height:16px;fill:currentColor}
  .voice-overlay{position:fixed;inset:0;z-index:150;background:rgba(0,0,0,.94);backdrop-filter:blur(30px);-webkit-backdrop-filter:blur(30px);display:none;flex-direction:column;justify-content:center;align-items:center;padding:30px;text-align:center}
  .voice-overlay.show{display:flex;animation:fade .25s ease}
  .voice-orb{width:140px;height:140px;border-radius:50%;background:linear-gradient(135deg,#00ffb4,#7c3aed);background-size:200% 200%;display:flex;align-items:center;justify-content:center;box-shadow:0 0 80px rgba(0,255,180,.5);animation:orbPulse 1.5s ease-in-out infinite,gradientShift 3s ease infinite;margin-bottom:30px;position:relative;flex-shrink:0}
  .voice-orb::before,.voice-orb::after{content:'';position:absolute;inset:0;border-radius:50%;background:linear-gradient(135deg,#00ffb4,#7c3aed);opacity:.3;z-index:-1}
  .voice-orb::before{animation:orbWave 2s ease-out infinite;animation-delay:.3s}
  .voice-orb::after{animation:orbWave 2s ease-out infinite}
  @keyframes orbWave{0%{transform:scale(1);opacity:.5}100%{transform:scale(2);opacity:0}}
  @keyframes orbPulse{0%,100%{transform:scale(1)}50%{transform:scale(1.06)}}
  .voice-orb svg{width:52px;height:52px;stroke:#000;fill:none;stroke-width:2.2;stroke-linecap:round;stroke-linejoin:round}
  .voice-status{font-size:16px;font-weight:700;color:#fff;margin-bottom:8px;font-family:'SF Mono',monospace}
  .voice-hint{font-size:12.5px;color:#8e8e93;max-width:280px;line-height:1.5}
  .voice-transcript{margin-top:22px;font-size:14px;color:#00ffb4;font-family:'SF Mono',monospace;max-width:340px;min-height:20px;line-height:1.5;font-style:italic}
  .voice-close{position:absolute;top:20px;right:18px;width:42px;height:42px;border-radius:50%;background:rgba(255,255,255,.08);border:none;color:#fff;display:flex;align-items:center;justify-content:center;cursor:pointer}
  .voice-close svg{width:19px;height:19px;stroke:#fff;fill:none;stroke-width:2.2;stroke-linecap:round}
  .menu{position:absolute;top:54px;right:12px;background:#1c1c1e;border:1px solid rgba(255,255,255,.08);border-radius:16px;padding:8px;min-width:250px;box-shadow:0 20px 60px rgba(0,0,0,.8);z-index:50;display:none;transform-origin:top right;max-height:70vh;overflow-y:auto}
  .menu.show{display:block;animation:dropIn .2s cubic-bezier(.2,.9,.3,1.2)}
  .menu::-webkit-scrollbar{width:0}
  @keyframes dropIn{from{opacity:0;transform:translateY(-8px) scale(.95)}to{opacity:1;transform:translateY(0) scale(1)}}
  .menu-item{display:flex;align-items:center;gap:12px;padding:11px 12px;border-radius:10px;color:#e5e5e7;font-size:13.5px;cursor:pointer;background:none;border:none;width:100%;text-align:left;font-weight:500;transition:.15s}
  .menu-item:active{background:rgba(255,255,255,.12)}
  .menu-item svg{width:17px;height:17px;stroke:#00ffb4;fill:none;stroke-width:1.9;stroke-linecap:round;stroke-linejoin:round;flex-shrink:0}
  .menu-sep{height:1px;background:rgba(255,255,255,.08);margin:6px 4px}
  .settings{position:fixed;inset:0;z-index:100;background:rgba(0,0,0,.75);backdrop-filter:blur(8px);-webkit-backdrop-filter:blur(8px);display:none;justify-content:center;align-items:flex-end}
  .settings.show{display:flex;animation:fade .2s ease}
  .settings-panel{width:100%;max-width:720px;background:#0f0f0f;border-radius:22px 22px 0 0;border:1px solid rgba(0,255,180,.15);border-bottom:none;padding:18px 18px calc(22px + env(safe-area-inset-bottom,0px));max-height:88vh;overflow-y:auto;animation:slideUp .3s cubic-bezier(.2,.9,.3,1.2);overscroll-behavior:contain}
  .settings-panel::-webkit-scrollbar{width:0}
  @keyframes slideUp{from{transform:translateY(100%)}to{transform:translateY(0)}}
  .settings-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:18px;padding-bottom:12px;border-bottom:1px solid rgba(255,255,255,.06);position:sticky;top:0;background:#0f0f0f;z-index:2}
  .settings-header h2{font-size:15px;font-weight:700;color:#00ffb4;font-family:'SF Mono',monospace;letter-spacing:.5px}
  .settings-close{width:32px;height:32px;border-radius:50%;background:#1c1c1e;border:none;color:#8e8e93;display:flex;align-items:center;justify-content:center;cursor:pointer}
  .settings-close svg{width:15px;height:15px;stroke:#8e8e93;stroke-width:2.2;fill:none;stroke-linecap:round}
  .settings-group{margin-bottom:18px}
  .settings-group h3{font-size:10px;font-weight:700;color:#7d8b99;letter-spacing:1.2px;text-transform:uppercase;margin-bottom:8px;font-family:'SF Mono',monospace}
  .setting-item{display:flex;align-items:center;justify-content:space-between;padding:11px 0;border-bottom:1px solid rgba(255,255,255,.05);gap:10px}
  .setting-item:last-child{border-bottom:none}
  .setting-item.col{flex-direction:column;align-items:stretch;gap:8px}
  .setting-info{flex:1;min-width:0}
  .setting-info strong{font-size:13px;color:#fff;font-weight:600;display:block}
  .setting-info span{font-size:10.5px;color:#7d8b99;margin-top:2px;display:block;line-height:1.3}
  .settings-input{background:#1c1c1e;border:1px solid rgba(255,255,255,.1);color:#fff;font-size:12px;padding:9px 11px;border-radius:9px;outline:none;width:100%;font-family:'SF Mono',monospace}
  .settings-input:focus{border-color:#00ffb4}
  .settings-select{background:#1c1c1e;border:1px solid rgba(255,255,255,.1);color:#fff;font-size:12px;padding:7px 10px;border-radius:9px;outline:none;flex-shrink:0;appearance:none;background-image:url("data:image/svg+xml;charset=UTF-8,%3Csvg width='12' height='12' viewBox='0 0 24 24' fill='none' stroke='%238e8e93' stroke-width='2.5' xmlns='http://www.w3.org/2000/svg'%3E%3Cpolyline points='6 9 12 15 18 9'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:right 8px center;padding-right:26px;font-family:'SF Mono',monospace;max-width:160px}
  .range-wrap{display:flex;align-items:center;gap:8px;flex-shrink:0}
  .range-wrap input[type=range]{-webkit-appearance:none;appearance:none;width:90px;height:5px;border-radius:3px;background:#2c2c2e;outline:none}
  .range-wrap input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;appearance:none;width:16px;height:16px;border-radius:50%;background:linear-gradient(135deg,#00ffb4,#7c3aed);cursor:pointer;border:none}
  .range-val{font-size:11px;color:#00ffb4;font-weight:600;min-width:38px;text-align:right;font-family:'SF Mono',monospace}
  .save-btn{width:100%;padding:13px;background:linear-gradient(135deg,#00ffb4,#00d9ff);border:none;border-radius:13px;color:#000;font-size:13.5px;font-weight:800;cursor:pointer;margin-top:6px;transition:.15s;font-family:'SF Mono',monospace;letter-spacing:.5px}
  .save-btn:active{transform:scale(.98)}
  .action-btn{width:100%;padding:12px;background:#1c1c1e;border:1px solid rgba(255,255,255,.1);border-radius:13px;color:#fff;font-size:13px;font-weight:600;cursor:pointer;margin-top:6px;transition:.15s;font-family:'SF Mono',monospace;display:flex;align-items:center;justify-content:center;gap:8px}
  .action-btn:active{transform:scale(.98);background:#2c2c2e}
  .action-btn.danger{color:#ff6b7a;border-color:rgba(255,107,122,.3)}
  .action-btn svg{width:15px;height:15px;stroke:currentColor;fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
  .endpoint-presets{display:flex;flex-wrap:wrap;gap:6px;margin-top:6px}
  .endpoint-preset{background:rgba(0,255,180,.08);border:1px solid rgba(0,255,180,.2);color:#00ffb4;font-size:10.5px;padding:5px 10px;border-radius:8px;cursor:pointer;font-family:'SF Mono',monospace;transition:.15s}
  .endpoint-preset:active{background:rgba(0,255,180,.2);transform:scale(.96)}
  .toast{position:fixed;bottom:100px;left:50%;transform:translateX(-50%) translateY(20px);background:#1c1c1e;color:#00ffb4;font-size:12.5px;padding:10px 18px;border-radius:12px;z-index:200;opacity:0;pointer-events:none;transition:.25s cubic-bezier(.2,.9,.3,1.2);white-space:nowrap;border:1px solid rgba(0,255,180,.2);font-family:'SF Mono',monospace;box-shadow:0 0 20px rgba(0,255,180,.2);max-width:90vw;text-align:center;overflow:hidden;text-overflow:ellipsis}
  .toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
  .toast.err{color:#f87171;border-color:rgba(239,68,68,.3);box-shadow:0 0 20px rgba(239,68,68,.2)}
  .camera-modal{position:fixed;inset:0;z-index:160;background:#000;display:none;flex-direction:column}
  .camera-modal.show{display:flex}
  .camera-modal video{flex:1;width:100%;object-fit:cover;background:#000}
  .camera-controls{position:absolute;bottom:30px;left:0;right:0;display:flex;justify-content:center;align-items:center;gap:30px;padding:0 30px}
  .camera-btn{width:70px;height:70px;border-radius:50%;background:#fff;border:4px solid rgba(255,255,255,.3);cursor:pointer;display:flex;align-items:center;justify-content:center;transition:.15s}
  .camera-btn:active{transform:scale(.9)}
  .camera-btn svg{width:28px;height:28px;stroke:#000;fill:none;stroke-width:2}
  .camera-close{position:absolute;top:20px;right:20px;width:44px;height:44px;border-radius:50%;background:rgba(0,0,0,.5);border:none;color:#fff;display:flex;align-items:center;justify-content:center;cursor:pointer}
  .camera-close svg{width:20px;height:20px;stroke:#fff;fill:none;stroke-width:2.4;stroke-linecap:round}
  .camera-flip{position:absolute;top:20px;left:20px;width:44px;height:44px;border-radius:50%;background:rgba(0,0,0,.5);border:none;color:#fff;display:flex;align-items:center;justify-content:center;cursor:pointer}
  .camera-flip svg{width:20px;height:20px;stroke:#fff;fill:none;stroke-width:2.2;stroke-linecap:round;stroke-linejoin:round}
  @media(max-width:400px){.welcome h2{font-size:19px}.msg{font-size:14px}.welcome-avatar{width:86px;height:86px;font-size:34px}.brand-text h1{font-size:12.5px}.voice-orb{width:120px;height:120px}.voice-orb svg{width:46px;height:46px}.msg.bot{max-width:94%}}
</style>
</head>
<body>
<div class="app" id="app">
  <div class="header">
    <div class="brand">
      <div class="brand-logo">
        <svg viewBox="0 0 24 24"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" y1="19" x2="12" y2="23"/><line x1="8" y1="23" x2="16" y2="23"/></svg>
      </div>
      <div class="brand-text">
        <h1>GrenX Voice Studio</h1>
        <p>chat • voice • video • edit foto</p>
      </div>
    </div>
    <div class="header-actions">
      <button class="btn-round" id="voiceModeBtn" title="Voice Mode">
        <svg viewBox="0 0 24 24"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" y1="19" x2="12" y2="23"/><line x1="8" y1="23" x2="16" y2="23"/></svg>
      </button>
      <button class="btn-round" id="settingsBtn" title="Pengaturan">
        <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
      </button>
      <button class="btn-round" id="menuBtn" title="Menu">
        <svg viewBox="0 0 24 24"><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="18" x2="21" y2="18"/></svg>
      </button>
    </div>
  </div>

  <div class="menu" id="menu">
    <button class="menu-item" onclick="menuAction('voice')"><svg viewBox="0 0 24 24"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" y1="19" x2="12" y2="23"/><line x1="8" y1="23" x2="16" y2="23"/></svg>Voice Mode</button>
    <button class="menu-item" onclick="menuAction('camera')"><svg viewBox="0 0 24 24"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"/><circle cx="12" cy="13" r="4"/></svg>Foto Live</button>
    <button class="menu-item" onclick="menuAction('video')"><svg viewBox="0 0 24 24"><polygon points="23 7 16 12 23 17 23 7"/><rect x="1" y="5" width="15" height="14" rx="2"/></svg>Generate Video</button>
    <button class="menu-item" onclick="menuAction('editFoto')"><svg viewBox="0 0 24 24"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>Edit Foto Asli</button>
    <button class="menu-item" onclick="menuAction('voiceSelect')"><svg viewBox="0 0 24 24"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" y1="19" x2="12" y2="23"/><line x1="8" y1="23" x2="16" y2="23"/></svg>Pilih Suara AI</button>
    <div class="menu-sep"></div>
    <button class="menu-item" onclick="menuAction('testVoice')"><svg viewBox="0 0 24 24"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"/><path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"/></svg>Test Suara</button>
    <button class="menu-item" onclick="menuAction('testApi')"><svg viewBox="0 0 24 24"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>Test Semua API</button>
    <button class="menu-item" onclick="menuAction('testEdit')"><svg viewBox="0 0 24 24"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>Test Edit Foto</button>
    <button class="menu-item" onclick="menuAction('clear')"><svg viewBox="0 0 24 24"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/></svg>Bersihkan Chat</button>
  </div>

  <div class="chat" id="chat">
    <div class="welcome" id="welcome">
      <div class="welcome-avatar">G</div>
      <h2>GrenX Voice Studio</h2>
      <p>Ngobrol santai, pakai suara, foto live, generate video, edit foto asli. Semua dalam satu.</p>
      <div class="welcome-chips">
        <div class="chip" onclick="quickAsk('hai, kenalan dong')">Ngobrol</div>
        <div class="chip" onclick="menuAction('voice')">Voice Mode</div>
        <div class="chip" onclick="menuAction('camera')">Foto Live</div>
        <div class="chip" onclick="menuAction('editFoto')">Edit Foto</div>
      </div>
    </div>
  </div>

  <div class="input-wrap" id="inputWrap">
    <div class="attach-bar" id="attachBar">
      <img id="attachImg" alt="attachment" />
      <div class="name" id="attachName">image.jpg</div>
      <button id="attachRemove">
        <svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
      </button>
    </div>
    <div class="input-bar" id="inputBar">
      <button class="ic-btn" id="uploadBtn" title="Lampirkan gambar">
        <svg viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><path d="M21 15l-5-5L5 21"/></svg>
      </button>
      <button class="ic-btn" id="cameraBtn" title="Foto Live">
        <svg viewBox="0 0 24 24"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"/><circle cx="12" cy="13" r="4"/></svg>
      </button>
      <textarea id="input" placeholder="Ketik pesan atau lampirkan foto + minta edit..." rows="1" enterkeyhint="send" autocomplete="off" autocorrect="off" spellcheck="false"></textarea>
      <button class="ic-btn" id="micBtn" title="Bicara">
        <svg viewBox="0 0 24 24"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" y1="19" x2="12" y2="23"/><line x1="8" y1="23" x2="16" y2="23"/></svg>
      </button>
      <button class="send-btn" id="send" title="Kirim">
        <svg viewBox="0 0 24 24"><path d="M2 21l21-9L2 3v7l15 2-15 2v7z"/></svg>
      </button>
    </div>
    <input type="file" id="fileInput" accept="image/*" style="display:none" />
  </div>

  <div class="toast" id="toast"></div>
</div>

<div class="voice-overlay" id="voiceOverlay">
  <button class="voice-close" id="voiceClose">
    <svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
  </button>
  <div class="voice-orb">
    <svg viewBox="0 0 24 24"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" y1="19" x2="12" y2="23"/><line x1="8" y1="23" x2="16" y2="23"/></svg>
  </div>
  <div class="voice-status" id="voiceStatus">Mendengarkan...</div>
  <div class="voice-hint" id="voiceHint">Bicara apa saja, AI akan menjawab dengan suara</div>
  <div class="voice-transcript" id="voiceTranscript"></div>
</div>

<div class="camera-modal" id="cameraModal">
  <video id="cameraVideo" autoplay playsinline muted></video>
  <button class="camera-close" id="cameraClose">
    <svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
  </button>
  <button class="camera-flip" id="cameraFlip">
    <svg viewBox="0 0 24 24"><polyline points="1 4 1 10 7 10"/><path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10"/></svg>
  </button>
  <div class="camera-controls">
    <button class="camera-btn" id="cameraCapture">
      <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"/></svg>
    </button>
  </div>
</div>

<div class="settings" id="settings">
  <div class="settings-panel">
    <div class="settings-header">
      <h2>&gt; PENGATURAN</h2>
      <button class="settings-close" id="settingsClose">
        <svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
      </button>
    </div>

    <div class="settings-group">
      <h3>✏️ Edit Foto (API Endpoint)</h3>
      <div class="setting-item col">
        <div class="setting-info">
          <strong>Endpoint Edit Foto</strong>
          <span>Base URL API (tanpa ?prompt=...&url=...)</span>
        </div>
        <input type="text" class="settings-input" id="setEditEndpoint" placeholder="http://api.ikyyxd.my.id/edit/gemini-flash" />
        <div class="endpoint-presets">
          <button class="endpoint-preset" onclick="setEditEndpointPreset('http://api.ikyyxd.my.id/edit/gemini-flash')">Gemini Flash</button>
          <button class="endpoint-preset" onclick="setEditEndpointPreset('http://api.ikyyxd.my.id/edit/nanobanana')">Nano Banana</button>
          <button class="endpoint-preset" onclick="setEditEndpointPreset('http://api.ikyyxd.my.id/edit/ai-edit-img')">AI Edit Img</button>
        </div>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Nama Parameter Foto</strong><span>Nama parameter URL untuk foto</span></div>
        <select class="settings-select" id="setEditParamName">
          <option value="url">url</option>
          <option value="image">image</option>
          <option value="img">img</option>
        </select>
      </div>
      <div style="padding:8px 10px;background:#0d1117;border:1px solid #1f2a36;border-radius:9px;font-family:'SF Mono',monospace;font-size:10.5px;color:#00ffb4;word-break:break-all" id="currentEndpointPreview">-</div>
    </div>

    <div class="settings-group">
      <h3>💬 Chat AI</h3>
      <div class="setting-item">
        <div class="setting-info"><strong>Endpoint Chat</strong><span>Sumber AI percakapan</span></div>
        <select class="settings-select" id="setEndpoint">
          <option value="auto">Auto</option>
          <option value="gpt4">Anabot GPT-4</option>
          <option value="qwen-https">IkyyXD Qwen (HTTPS)</option>
          <option value="qwen-http">IkyyXD Qwen (HTTP)</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Timeout</strong><span>Waktu tunggu respon</span></div>
        <div class="range-wrap">
          <input type="range" id="setTimeout" min="10" max="120" value="30">
          <span class="range-val" id="setTimeoutVal">30s</span>
        </div>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Ukuran Font Chat</strong><span>Besar teks pesan</span></div>
        <div class="range-wrap">
          <input type="range" id="setFontSize" min="12" max="20" value="15">
          <span class="range-val" id="setFontSizeVal">15px</span>
        </div>
      </div>
    </div>

    <div class="settings-group">
      <h3>🔊 Voice</h3>
      <div class="setting-item">
        <div class="setting-info"><strong>Auto-Speak</strong><span>AI balas dengan suara</span></div>
        <label class="switch"><input type="checkbox" id="setSpeak" checked><span class="slider"></span></label>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Voice Mode Loop</strong><span>Dengar → jawab → dengar lagi</span></div>
        <label class="switch"><input type="checkbox" id="setLoop" checked><span class="slider"></span></label>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Pilih Suara AI</strong><span>Karakter suara</span></div>
        <select class="settings-select" id="setVoicePreset">
          <option value="default">Default</option>
          <option value="friendly">Ramah</option>
          <option value="cheerful">Ceria</option>
          <option value="calm">Tenang</option>
          <option value="serious">Tegas</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Bahasa Suara</strong><span>Aksen TTS</span></div>
        <select class="settings-select" id="setLang">
          <option value="id-ID">Indonesia</option>
          <option value="en-US">English US</option>
          <option value="en-GB">English UK</option>
          <option value="ja-JP">日本語</option>
          <option value="ko-KR">한국어</option>
          <option value="ar-SA">العربية</option>
          <option value="zh-CN">中文</option>
          <option value="es-ES">Español</option>
          <option value="fr-FR">Français</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Kecepatan</strong><span>Speed suara</span></div>
        <div class="range-wrap">
          <input type="range" id="setRate" min="5" max="20" value="10">
          <span class="range-val" id="setRateVal">1.0x</span>
        </div>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Nada</strong><span>Pitch suara</span></div>
        <div class="range-wrap">
          <input type="range" id="setPitch" min="5" max="20" value="10">
          <span class="range-val" id="setPitchVal">1.0</span>
        </div>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Volume</strong><span>Keras suara</span></div>
        <div class="range-wrap">
          <input type="range" id="setVolume" min="0" max="10" value="10">
          <span class="range-val" id="setVolumeVal">100%</span>
        </div>
      </div>
    </div>

    <div class="settings-group">
      <h3>🎨 Media</h3>
      <div class="setting-item">
        <div class="setting-info"><strong>Kualitas Video</strong><span>Resolusi video</span></div>
        <select class="settings-select" id="setVidQuality">
          <option value="16:9">16:9 Landscape</option>
          <option value="9:16">9:16 Portrait</option>
          <option value="1:1">1:1 Square</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Kamera Default</strong><span>Depan / belakang</span></div>
        <select class="settings-select" id="setCameraFacing">
          <option value="environment">Belakang</option>
          <option value="user">Depan</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Auto-Save Foto</strong><span>Simpan otomatis ke chat</span></div>
        <label class="switch"><input type="checkbox" id="setAutoSave" checked><span class="slider"></span></label>
      </div>
    </div>

    <div class="settings-group">
      <h3>🎭 Tampilan</h3>
      <div class="setting-item">
        <div class="setting-info"><strong>Tema</strong><span>Mode warna</span></div>
        <select class="settings-select" id="setTheme">
          <option value="dark">Gelap</option>
          <option value="light">Terang</option>
          <option value="auto">Auto</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info"><strong>Compact Mode</strong><span>Pesan lebih rapat</span></div>
        <label class="switch"><input type="checkbox" id="setCompact"><span class="slider"></span></label>
      </div>
    </div>

    <div class="settings-group">
      <h3>💾 Data</h3>
      <button class="action-btn" id="exportSettings">
        <svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
        Export Pengaturan
      </button>
      <button class="action-btn" id="importSettings">
        <svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
        Import Pengaturan
      </button>
      <button class="action-btn danger" id="resetSettings">
        <svg viewBox="0 0 24 24"><polyline points="1 4 1 10 7 10"/><path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10"/></svg>
        Reset ke Default
      </button>
      <input type="file" id="importFile" accept="application/json" style="display:none" />
    </div>

    <button class="save-btn" id="saveSettings">SIMPAN PENGATURAN</button>
  </div>
</div>

<script>
/* ============ KONFIG ============ */
const ENDPOINTS = [
  { url: 'https://anabot.my.id/api/ai/gpt4', param: 'prompt', key: 'freeApikey', type: 'gpt4' },
  { url: 'https://api.ikyyxd.my.id/ai/qwen', param: 'prompt', key: null, type: 'qwen' },
  { url: 'http://api.ikyyxd.my.id/ai/qwen', param: 'prompt', key: null, type: 'qwen' }
];

const VIDEO_API = 'https://api.theresav.eu/api/video/qwen/i2v';

const DEFAULT_EDIT_ENDPOINT = 'http://api.ikyyxd.my.id/edit/gemini-flash';
const DEFAULT_EDIT_PARAM = 'url';

const EDIT_ACTIONS = [
  'edit foto','edit gambar','ubah foto','ubah gambar','ganti background',
  'hapus background','hilangkan background','retouch','perbaiki foto',
  'ubah jadi','ganti jadi','jadi anime','jadi kartun','jadi hitam putih',
  'kasih filter','tambah efek','edit wajah','jangan ubah wajah','jangan ubah muka',
  'edit ini','edit gambar ini','edit foto ini','edit','ubah','ganti','ubah gayanya'
];

const VIDEO_ACTIONS = [
  'buat video','buatkan video','bikin video','generate video',
  'buat animasi','bikin animasi','video ','animate '
];

function isVideoRequest(t){
  const lower = t.toLowerCase().trim();
  return VIDEO_ACTIONS.some(k => lower.includes(k));
}
function isEditRequest(t){
  const lower = t.toLowerCase().trim();
  return EDIT_ACTIONS.some(w => lower.includes(w));
}

const VOICE_PRESETS = {
  default:  { rate: 1.00, pitch: 1.00 },
  friendly: { rate: 1.02, pitch: 1.10 },
  cheerful: { rate: 1.10, pitch: 1.20 },
  calm:     { rate: 0.92, pitch: 0.92 },
  serious:  { rate: 0.98, pitch: 0.85 }
};

/* ============ SETTINGS ============ */
const DEFAULTS = {
  endpoint: 'auto',
  timeout: 30,
  fontSize: 15,
  speak: true,
  loop: true,
  voicePreset: 'default',
  lang: 'id-ID',
  rate: 10,
  pitch: 10,
  volume: 10,
  vidQuality: '16:9',
  cameraFacing: 'environment',
  autoSave: true,
  theme: 'dark',
  compact: false,
  editEndpoint: DEFAULT_EDIT_ENDPOINT,
  editParamName: DEFAULT_EDIT_PARAM
};
let settings = { ...DEFAULTS };
try { Object.assign(settings, JSON.parse(localStorage.getItem('voice-settings') || '{}')); } catch {}
function persist(){ try { localStorage.setItem('voice-settings', JSON.stringify(settings)); } catch {} }
persist();

/* ============ ELEMENT ============ */
const chat = document.getElementById('chat');
const input = document.getElementById('input');
const sendBtn = document.getElementById('send');
const toast = document.getElementById('toast');
const uploadBtn = document.getElementById('uploadBtn');
const cameraBtn = document.getElementById('cameraBtn');
const fileInput = document.getElementById('fileInput');
const inputBar = document.getElementById('inputBar');
const menu = document.getElementById('menu');
const menuBtn = document.getElementById('menuBtn');
const settingsBtn = document.getElementById('settingsBtn');
const voiceModeBtn = document.getElementById('voiceModeBtn');
const settingsPanel = document.getElementById('settings');
const settingsClose = document.getElementById('settingsClose');
const micBtn = document.getElementById('micBtn');
const voiceOverlay = document.getElementById('voiceOverlay');
const voiceClose = document.getElementById('voiceClose');
const voiceStatus = document.getElementById('voiceStatus');
const voiceHint = document.getElementById('voiceHint');
const voiceTranscript = document.getElementById('voiceTranscript');
const attachBar = document.getElementById('attachBar');
const attachImg = document.getElementById('attachImg');
const attachName = document.getElementById('attachName');
const attachRemove = document.getElementById('attachRemove');
const cameraModal = document.getElementById('cameraModal');
const cameraVideo = document.getElementById('cameraVideo');
const cameraClose = document.getElementById('cameraClose');
const cameraFlip = document.getElementById('cameraFlip');
const cameraCapture = document.getElementById('cameraCapture');

let isBusy = false;
let attachedImage = null;
let recognition = null;
let listening = false;
let finalizeTimer = null;
let lastTranscript = '';
let speechUnlocked = false;
let cameraStream = null;
let cameraFacing = settings.cameraFacing;
let loopMode = settings.loop;
let inVoiceOverlay = false;
let micPermissionGranted = false;

function logDebug(label, data) {
  console.log('%c[GrenX] ' + label, 'color:#00ffb4;font-weight:bold', data || '');
}

/* ============ UNLOCK SPEECH ============ */
function unlockSpeech() {
  if (speechUnlocked) return;
  speechUnlocked = true;
  try {
    const u = new SpeechSynthesisUtterance('');
    u.volume = 0;
    window.speechSynthesis.speak(u);
    window.speechSynthesis.cancel();
  } catch (e) {}
}
document.addEventListener('touchstart', unlockSpeech, { once: true });
document.addEventListener('click', unlockSpeech, { once: true });
document.addEventListener('keydown', unlockSpeech, { once: true });

/* ============ BLOCK GESTURES ============ */
document.addEventListener('gesturestart', e => e.preventDefault());
document.addEventListener('gesturechange', e => e.preventDefault());
document.addEventListener('gestureend', e => e.preventDefault());
document.addEventListener('contextmenu', e => e.preventDefault());
let lastTouchEnd = 0;
document.addEventListener('touchend', e => {
  const now = Date.now();
  if (now - lastTouchEnd <= 300) e.preventDefault();
  lastTouchEnd = now;
}, { passive:false });

/* ============ TOAST ============ */
function showToast(msg, isError = false) {
  toast.textContent = msg;
  toast.className = 'toast show' + (isError ? ' err' : '');
  clearTimeout(toast._t);
  toast._t = setTimeout(() => toast.classList.remove('show'), 2200);
}

/* ============ THEME ============ */
function applyTheme() {
  const t = settings.theme;
  if (t === 'light') { document.body.style.background = '#f5f5f7'; document.body.style.color = '#000'; }
  else if (t === 'auto') {
    const dark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    document.body.style.background = dark ? '#000' : '#f5f5f7';
    document.body.style.color = dark ? '#fff' : '#000';
  } else { document.body.style.background = '#000'; document.body.style.color = '#fff'; }
}
applyTheme();

function applyFontSize() {
  document.querySelectorAll('.msg').forEach(m => m.style.fontSize = settings.fontSize + 'px');
}
function applyCompact() {
  const pad = settings.compact ? '8px 10px' : '11px 14px';
  document.querySelectorAll('.msg').forEach(m => m.style.padding = pad);
}
function hideWelcome(){ const w = document.getElementById('welcome'); if (w) w.remove(); }

function syncViewport() {
  requestAnimationFrame(() => {
    const isNearBottom = chat.scrollHeight - chat.scrollTop - chat.clientHeight < 200;
    if (isNearBottom) chat.scrollTop = chat.scrollHeight;
  });
}
window.addEventListener('resize', syncViewport);
window.addEventListener('orientationchange', () => setTimeout(syncViewport, 250));
if (window.visualViewport) {
  window.visualViewport.addEventListener('resize', syncViewport);
  window.visualViewport.addEventListener('scroll', syncViewport);
}
syncViewport();

/* ============ MIC PERMISSION ============ */
async function requestMicPermission() {
  if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
    showToast('Browser tidak support mikrofon', true);
    return false;
  }
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    stream.getTracks().forEach(t => t.stop());
    micPermissionGranted = true;
    return true;
  } catch (e) {
    if (e.name === 'NotAllowedError') showToast('Izin mikrofon ditolak. Buka pengaturan browser → izinkan mikrofon', true);
    else if (e.name === 'NotFoundError') showToast('Tidak ada mikrofon terdeteksi', true);
    else showToast('Gagal akses mikrofon: ' + e.message, true);
    return false;
  }
}

/* ============ TTS ============ */
function speak(text, onDone) {
  if (!settings.speak) { if (onDone) onDone(); return; }
  if (!('speechSynthesis' in window)) { showToast('Browser tidak support suara', true); if (onDone) onDone(); return; }
  if (!speechUnlocked) unlockSpeech();
  try {
    window.speechSynthesis.cancel();
    let clean = String(text)
      .replace(/```[\s\S]*?```/g, ' kode program ')
      .replace(/[*_~`#>|]/g, '')
      .replace(/https?:\/\/\S+/g, ' link ')
      .replace(/[\u{1F300}-\u{1FAFF}\u{2600}-\u{27BF}]/gu, '')
      .replace(/\s+/g, ' ').trim();
    if (!clean) { if (onDone) onDone(); return; }
    if (clean.length > 700) clean = clean.slice(0, 700) + '.';
    clean = clean.replace(/([.!?])\s+/g, '$1  ');
    const u = new SpeechSynthesisUtterance(clean);
    const preset = VOICE_PRESETS[settings.voicePreset] || VOICE_PRESETS.default;
    u.lang = settings.lang;
    u.rate = ((settings.rate || 10) / 10) * preset.rate;
    u.pitch = ((settings.pitch || 10) / 10) * preset.pitch;
    u.volume = (settings.volume || 10) / 10;
    const voices = window.speechSynthesis.getVoices();
    const langPrefix = settings.lang.split('-')[0];
    const exact = voices.find(v => v.lang === settings.lang);
    const prefix = voices.find(v => v.lang.startsWith(langPrefix));
    const def = voices.find(v => v.default);
    const chosen = exact || prefix || def || voices[0];
    if (chosen) { u.voice = chosen; u.lang = chosen.lang; }
    let doneCalled = false;
    const done = () => {
      if (doneCalled) return;
      doneCalled = true;
      micBtn.classList.remove('mic-active');
      if (onDone) onDone();
    };
    u.onstart = () => micBtn.classList.add('mic-active');
    u.onend = done;
    u.onerror = done;
    window.speechSynthesis.speak(u);
    setTimeout(() => { if (window.speechSynthesis.paused) window.speechSynthesis.resume(); }, 100);
  } catch (e) { console.error('[TTS]', e); if (onDone) onDone(); }
}
function stopSpeak() {
  try { window.speechSynthesis.cancel(); micBtn.classList.remove('mic-active'); } catch {}
}

/* ============ ATTACH ============ */
uploadBtn.addEventListener('click', () => fileInput.click());
fileInput.addEventListener('change', () => {
  const f = fileInput.files?.[0];
  if (!f) return;
  const reader = new FileReader();
  reader.onload = (ev) => {
    attachedImage = { dataUrl: ev.target.result, name: f.name };
    attachImg.src = ev.target.result;
    attachName.textContent = f.name;
    attachBar.classList.add('show');
    showToast('Gambar dilampirkan');
  };
  reader.readAsDataURL(f);
  fileInput.value = '';
});
attachRemove.addEventListener('click', () => {
  attachedImage = null;
  attachBar.classList.remove('show');
});

/* ============ CAMERA LIVE ============ */
async function openCamera() {
  try {
    cameraFacing = settings.cameraFacing || 'environment';
    cameraStream = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: cameraFacing },
      audio: false
    });
    cameraVideo.srcObject = cameraStream;
    cameraModal.classList.add('show');
  } catch (e) { showToast('Gagal buka kamera: ' + e.message, true); }
}
function closeCamera() {
  cameraModal.classList.remove('show');
  if (cameraStream) { cameraStream.getTracks().forEach(t => t.stop()); cameraStream = null; }
  cameraVideo.srcObject = null;
}
function capturePhoto() {
  const canvas = document.createElement('canvas');
  canvas.width = cameraVideo.videoWidth;
  canvas.height = cameraVideo.videoHeight;
  const ctx = canvas.getContext('2d');
  ctx.drawImage(cameraVideo, 0, 0);
  const dataUrl = canvas.toDataURL('image/jpeg', 0.9);
  if (settings.autoSave) {
    attachedImage = { dataUrl, name: 'camera_' + Date.now() + '.jpg' };
    attachImg.src = dataUrl;
    attachName.textContent = attachedImage.name;
    attachBar.classList.add('show');
    showToast('Foto diambil & dilampirkan');
  } else {
    const a = document.createElement('a');
    a.href = dataUrl;
    a.download = 'grenx_' + Date.now() + '.jpg';
    a.click();
    showToast('Foto disimpan');
  }
  closeCamera();
}
cameraBtn.addEventListener('click', openCamera);
cameraClose.addEventListener('click', closeCamera);
cameraCapture.addEventListener('click', capturePhoto);
cameraFlip.addEventListener('click', async () => {
  cameraFacing = cameraFacing === 'environment' ? 'user' : 'environment';
  if (cameraStream) {
    cameraStream.getTracks().forEach(t => t.stop());
    try {
      cameraStream = await navigator.mediaDevices.getUserMedia({
        video: { facingMode: cameraFacing },
        audio: false
      });
      cameraVideo.srcObject = cameraStream;
    } catch (e) { showToast('Gagal flip kamera', true); }
  }
});

/* ============ FORMAT ============ */
function formatReply(text) {
  const esc = s => String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
  const parts = String(text).split(/```/);
  let html = '';
  parts.forEach((p, i) => {
    if (i % 2 === 1) {
      const lines = p.split('\n');
      const first = lines[0].trim();
      const isLang = /^[a-zA-Z0-9+#-]+$/.test(first);
      const code = (isLang ? lines.slice(1).join('\n') : p).replace(/^\n+|\n+$/g, '');
      html += `<pre><code>${esc(code)}</code></pre>`;
    } else {
      html += esc(p).replace(/`([^`]+)`/g, '<code>$1</code>');
    }
  });
  return html;
}
const scrollBottom = () => requestAnimationFrame(() => { chat.scrollTop = chat.scrollHeight; });

function addMessage(text, type='bot', attach=null) {
  hideWelcome();
  const d = document.createElement('div');
  d.className = 'msg ' + type;
  d.style.fontSize = settings.fontSize + 'px';
  if (settings.compact) d.style.padding = '8px 10px';
  if (type === 'bot') {
    d.innerHTML = `<span class="label"><span class="dot">G</span>GrenX Studio</span>${formatReply(text)}`;
    const speakBtn = document.createElement('button');
    speakBtn.className = 'speak-btn';
    speakBtn.innerHTML = '<svg viewBox="0 0 24 24"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"/><path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"/></svg> Putar Suara';
    speakBtn.onclick = () => speak(text);
    d.appendChild(speakBtn);
  } else {
    d.textContent = text;
    if (attach) {
      const img = document.createElement('img');
      img.className = 'att';
      img.src = attach;
      d.appendChild(img);
    }
  }
  chat.appendChild(d);
  scrollBottom();
  return d;
}
function addTyping() {
  const d = document.createElement('div');
  d.className = 'typing';
  d.id = 'typing';
  d.innerHTML = '<span></span><span></span><span></span>';
  chat.appendChild(d);
  scrollBottom();
}
function rmTyping(){ const t = document.getElementById('typing'); if (t) t.remove(); }

/* ============ EDIT PROGRESS ============ */
function createEditProgress() {
  hideWelcome();
  const d = document.createElement('div');
  d.className = 'msg bot';
  d.id = 'editProgress';
  d.innerHTML = `
    <span class="label"><span class="dot">G</span>Proses Edit Foto</span>
    <div class="edit-steps">
      <div class="edit-step" data-step="upload"><div class="ico">1</div><span>Upload foto ke server</span></div>
      <div class="edit-step" data-step="request"><div class="ico">2</div><span>Proses dengan AI</span></div>
      <div class="edit-step" data-step="render"><div class="ico">3</div><span>Tampilkan hasil</span></div>
    </div>
    <div class="edit-timer" id="editTimer">⏱️ 0s</div>
  `;
  chat.appendChild(d);
  scrollBottom();
  const startTime = Date.now();
  const timerEl = d.querySelector('#editTimer');
  const timerInt = setInterval(() => {
    if (!document.body.contains(d)) { clearInterval(timerInt); return; }
    const s = Math.floor((Date.now() - startTime) / 1000);
    if (timerEl) timerEl.textContent = '⏱️ ' + s + 's';
  }, 500);
  return d;
}
function setEditStep(stepName, state) {
  const bubble = document.getElementById('editProgress');
  if (!bubble) return;
  const el = bubble.querySelector(`.edit-step[data-step="${stepName}"]`);
  if (!el) return;
  el.classList.remove('active', 'done');
  const ico = el.querySelector('.ico');
  ico.classList.remove('spin');
  if (state === 'active') { el.classList.add('active'); ico.classList.add('spin'); ico.textContent = ''; }
  else if (state === 'done') { el.classList.add('done'); ico.classList.remove('spin'); ico.textContent = '✓'; }
}
function finishEditProgress() {
  const bubble = document.getElementById('editProgress');
  if (bubble) {
    bubble.style.transition = 'opacity .3s';
    bubble.style.opacity = '0';
    setTimeout(() => bubble.remove(), 300);
  }
}

/* ============ ADD IMAGE ============ */
function addImageMessage(imageUrl, promptText) {
  hideWelcome();
  const d = document.createElement('div');
  d.className = 'msg bot';
  d.innerHTML = `<span class="label"><span class="dot">G</span>Hasil Edit</span>`;
  const wrap = document.createElement('div');
  wrap.className = 'img-wrap';
  const loader = document.createElement('div');
  loader.className = 'img-loader';
  loader.innerHTML = `
    <div class="img-spin"></div>
    <span id="imgStatusText">Memuat gambar... 0%</span>
  `;
  const img = document.createElement('img');
  img.className = 'gen';
  img.alt = promptText;
  img.style.display = 'none';
  img.loading = 'eager';
  let done = false;
  let pct = 0;
  const statusText = () => loader.querySelector('#imgStatusText');
  const progressTimer = setInterval(() => {
    if (done) return;
    pct = Math.min(pct + Math.random() * 7, 92);
    const el = statusText();
    if (el) el.textContent = `Memuat gambar... ${Math.floor(pct)}%`;
  }, 800);
  const finish = (ok, errMsg) => {
    done = true;
    clearInterval(progressTimer);
    if (ok) {
      loader.remove();
      img.style.display = 'block';
      scrollBottom();
    } else {
      loader.classList.remove('img-loader');
      loader.classList.add('img-fallback');
      loader.innerHTML = `
        <div class="icon">⚠️</div>
        <div class="text">${errMsg || 'Gagal memuat gambar'}</div>
        <button onclick="window.open('${imageUrl}','_blank')">Buka di Tab Baru</button>
      `;
    }
  };
  img.onload = () => finish(true);
  img.onerror = () => {
    if (!img.dataset.retried) {
      img.dataset.retried = '1';
      if (statusText()) statusText().textContent = 'Retry...';
      setTimeout(() => { img.src = imageUrl + '?t=' + Date.now(); }, 400);
      return;
    }
    finish(false, 'Gambar gagal dimuat');
  };
  img.onclick = () => window.open(img.src, '_blank');
  wrap.appendChild(loader);
  wrap.appendChild(img);
  d.appendChild(wrap);
  chat.appendChild(d);
  scrollBottom();
  img.src = imageUrl;
  setTimeout(() => { if (!done) finish(false, 'Timeout — server lambat'); }, 60000);
}

/* ============ UPLOAD FOTO (Catbox Priority) ============ */
async function uploadPhoto(dataUrl) {
  logDebug('Upload: konversi ke blob');
  const res = await fetch(dataUrl);
  const blob = await res.blob();
  logDebug('Upload: blob size', blob.size + ' bytes');

  const fileName = 'grenx_' + Date.now() + '.jpg';
  const errors = [];

  // PRIORITAS 1: catbox.moe
  try {
    logDebug('Upload [1/3]: catbox.moe');
    const file = new File([blob], fileName, { type: 'image/jpeg' });
    const form = new FormData();
    form.append('reqtype', 'fileupload');
    form.append('fileToUpload', file);
    const ctrl = new AbortController();
    const timer = setTimeout(() => ctrl.abort(), 30000);
    const upRes = await fetch('https://catbox.moe/user/api.php', { method: 'POST', body: form, signal: ctrl.signal });
    clearTimeout(timer);
    if (upRes.ok) {
      const url = (await upRes.text()).trim();
      logDebug('Upload catbox response:', url);
      if (url.startsWith('http')) {
        logDebug('Upload [1] OK', url);
        return url;
      }
    }
    errors.push('catbox: HTTP ' + upRes.status);
  } catch (e) {
    logDebug('Upload [1] gagal', e.message);
    errors.push('catbox: ' + e.message);
  }

  // PRIORITAS 2: tmpfiles.org
  try {
    logDebug('Upload [2/3]: tmpfiles.org');
    const file = new File([blob], fileName, { type: 'image/jpeg' });
    const form = new FormData();
    form.append('file', file);
    const ctrl = new AbortController();
    const timer = setTimeout(() => ctrl.abort(), 30000);
    const upRes = await fetch('https://tmpfiles.org/api/v1/upload', { method: 'POST', body: form, signal: ctrl.signal });
    clearTimeout(timer);
    if (upRes.ok) {
      const data = await upRes.json();
      const rawUrl = data?.data?.url;
      if (rawUrl && rawUrl.startsWith('http')) {
        const directUrl = rawUrl.replace('tmpfiles.org/', 'tmpfiles.org/dl/');
        logDebug('Upload [2] OK', directUrl);
        return directUrl;
      }
    }
    errors.push('tmpfiles: HTTP ' + upRes.status);
  } catch (e) {
    logDebug('Upload [2] gagal', e.message);
    errors.push('tmpfiles: ' + e.message);
  }

  // PRIORITAS 3: uguu.se
  try {
    logDebug('Upload [3/3]: uguu.se');
    const file = new File([blob], fileName, { type: 'image/jpeg' });
    const form = new FormData();
    form.append('files[]', file);
    const ctrl = new AbortController();
    const timer = setTimeout(() => ctrl.abort(), 30000);
    const upRes = await fetch('https://uguu.se/upload.php', { method: 'POST', body: form, signal: ctrl.signal });
    clearTimeout(timer);
    if (upRes.ok) {
      const data = await upRes.json();
      const url = data?.files?.[0]?.url;
      if (url && url.startsWith('http')) {
        logDebug('Upload [3] OK', url);
        return url;
      }
    }
    errors.push('uguu: HTTP ' + upRes.status);
  } catch (e) {
    logDebug('Upload [3] gagal', e.message);
    errors.push('uguu: ' + e.message);
  }

  throw new Error('Semua server upload gagal. Detail: ' + errors.join(' | '));
}

/* ============ EDIT FOTO (Multi-Encoding Fallback) ============ */
async function editPhotoApi(imageUrl, prompt) {
  const endpoint = settings.editEndpoint || DEFAULT_EDIT_ENDPOINT;
  const paramName = settings.editParamName || DEFAULT_EDIT_PARAM;

  // 3 variasi encoding
  const variations = [
    { name: 'encodeURIComponent (standar)', url: encodeURIComponent(imageUrl), prompt: encodeURIComponent(prompt) },
    { name: 'encodeURI (lebih ringan)',     url: encodeURI(imageUrl),          prompt: encodeURI(prompt) },
    { name: 'raw (tanpa encoding)',         url: imageUrl,                     prompt: prompt }
  ];

  const maxRetriesPerVariation = 2;
  let lastErr = null;

  for (const variation of variations) {
    logDebug(`=== Coba encoding: ${variation.name} ===`);
    const separator = endpoint.includes('?') ? '&' : '?';
    const apiUrl = `${endpoint}${separator}${paramName}=${variation.url}&prompt=${variation.prompt}`;

    logDebug('Image URL asli:', imageUrl);
    logDebug('Encoded:', variation.url);
    logDebug('API URL final:', apiUrl);
    logDebug('Prompt:', prompt);

    for (let attempt = 1; attempt <= maxRetriesPerVariation; attempt++) {
      logDebug(`Attempt ${attempt}/${maxRetriesPerVariation} untuk ${variation.name}`);

      const ctrl = new AbortController();
      const timer = setTimeout(() => ctrl.abort(), 60000);

      try {
        const res = await fetch(apiUrl, { method: 'GET', signal: ctrl.signal });
        clearTimeout(timer);

        logDebug(`HTTP ${res.status}`);
        const text = await res.text();
        logDebug('Response preview:', text.slice(0, 300));

        if (res.ok) {
          let data;
          try { data = JSON.parse(text); }
          catch (e) {
            if (text.trim().startsWith('http')) {
              logDebug('SUKSES — response langsung URL');
              return text.trim();
            }
            lastErr = new Error('Response bukan JSON & bukan URL: ' + text.slice(0, 100));
            continue;
          }

          const resultUrl =
            data?.result?.url || data?.result ||
            data?.url || data?.image ||
            data?.data?.url || data?.data?.image ||
            data?.data?.result?.url || data?.data?.result ||
            (typeof data === 'string' ? data : null);

          if (resultUrl && String(resultUrl).startsWith('http')) {
            logDebug(`SUKSES dengan ${variation.name}`, resultUrl);
            return String(resultUrl);
          }

          if (data?.status === false) {
            lastErr = new Error('Server error: ' + (data?.message?.error || data?.message || 'unknown'));
          } else {
            lastErr = new Error('Response tidak berisi URL: ' + JSON.stringify(data).slice(0, 150));
          }
        } else {
          const errPreview = text.slice(0, 150);
          lastErr = new Error(`HTTP ${res.status}: ${errPreview}`);
          logDebug(`Attempt ${attempt} gagal:`, errPreview);

          if (attempt < maxRetriesPerVariation) {
            await new Promise(r => setTimeout(r, 1500));
            continue;
          }
        }
      } catch (e) {
        clearTimeout(timer);
        if (e.name === 'AbortError') {
          lastErr = new Error('Timeout 60 detik');
        } else if (String(e.message).includes('Failed to fetch') || String(e.message).includes('NetworkError')) {
          lastErr = new Error('BLOCKED — kemungkinan Mixed Content (HTTPS page + HTTP API)');
        } else {
          lastErr = e;
        }
        logDebug(`Attempt ${attempt} error:`, e.message);

        if (attempt < maxRetriesPerVariation) {
          await new Promise(r => setTimeout(r, 1500));
          continue;
        }
      }
    }
    logDebug(`Variasi "${variation.name}" gagal, coba variasi berikutnya...`);
  }

  throw lastErr || new Error('Semua variasi encoding gagal');
}

/* ============ VIDEO GEN ============ */
async function generateVideo(text) {
  const clean = text.replace(/^(buatkan|bikin|buat|generate)\s+(video|animasi|animation)\s*/i, '').trim();
  const prompt = clean || text;
  return { url: `${VIDEO_API}?prompt=${encodeURIComponent(prompt)}&size=${settings.vidQuality}`, prompt };
}
function addVideoMessage(videoUrl, promptText) {
  hideWelcome();
  const d = document.createElement('div');
  d.className = 'msg bot';
  d.innerHTML = `<span class="label"><span class="dot">G</span>Generated Video</span>`;
  const vid = document.createElement('video');
  vid.className = 'gen';
  vid.src = videoUrl;
  vid.controls = true;
  vid.autoplay = true;
  vid.loop = true;
  vid.muted = true;
  vid.playsInline = true;
  d.appendChild(vid);
  chat.appendChild(d);
  scrollBottom();
}

/* ============ PARSER & FETCH ============ */
function parseChat(data, type) {
  if (!data) return '';
  if (type === 'qwen') return data?.result?.response || data?.result?.message || '';
  return data?.result || data?.response || data?.message || data?.answer || data?.data?.message || '';
}
async function fetchWithTimeout(url, timeoutMs) {
  const ctrl = new AbortController();
  const timer = setTimeout(() => ctrl.abort(), timeoutMs);
  try {
    const res = await fetch(url, { method: 'GET', signal: ctrl.signal, mode: 'cors' });
    clearTimeout(timer);
    return res;
  } catch (e) { clearTimeout(timer); throw e; }
}

async function callChat(promptText) {
  const timeoutMs = (settings.timeout || 30) * 1000;
  let endpoints;
  if (settings.endpoint === 'auto') endpoints = ENDPOINTS;
  else if (settings.endpoint === 'gpt4') endpoints = ENDPOINTS.filter(e => e.type === 'gpt4');
  else if (settings.endpoint === 'qwen-https') endpoints = ENDPOINTS.filter(e => e.type === 'qwen' && e.url.startsWith('https'));
  else if (settings.endpoint === 'qwen-http') endpoints = ENDPOINTS.filter(e => e.type === 'qwen' && e.url.startsWith('http://'));
  else endpoints = ENDPOINTS;
  if (!endpoints.length) endpoints = ENDPOINTS;

  let lastErr = null;
  for (const ep of endpoints) {
    try {
      const url = `${ep.url}?${ep.param}=${encodeURIComponent(promptText)}${ep.key ? '&apikey=' + ep.key : ''}`;
      const res = await fetchWithTimeout(url, timeoutMs);
      if (!res.ok) throw new Error('HTTP ' + res.status);
      let data;
      const text = await res.text();
      try { data = JSON.parse(text); }
      catch (e) {
        if (text && text.length > 1 && !text.startsWith('<')) return text.trim();
        throw new Error('Response bukan JSON');
      }
      const result = parseChat(data, ep.type);
      if (result && String(result).trim()) return String(result).trim();
      lastErr = new Error('Response kosong');
    } catch (e) { lastErr = e; continue; }
  }
  throw lastErr || new Error('Semua endpoint gagal');
}

/* ============ TEST ============ */
async function testAllApis() {
  const testPrompt = 'halo';
  const results = [];
  for (const ep of ENDPOINTS) {
    const url = `${ep.url}?${ep.param}=${encodeURIComponent(testPrompt)}${ep.key ? '&apikey=' + ep.key : ''}`;
    try {
      const res = await fetchWithTimeout(url, 10000);
      if (!res.ok) { results.push(`FAIL ${ep.url}: HTTP ${res.status}`); continue; }
      const text = await res.text();
      let data; try { data = JSON.parse(text); } catch { data = { result: text }; }
      const result = parseChat(data, ep.type);
      if (result && String(result).trim()) results.push(`OK   ${ep.url}`);
      else results.push(`WARN ${ep.url}: kosong`);
    } catch (e) { results.push(`FAIL ${ep.url}: ${e.message}`); }
  }
  addMessage('**Hasil Test Semua API:**\n\n```\n' + results.join('\n') + '\n```', 'bot');
}

async function testEditApi() {
  const testImgUrl = 'https://files.catbox.moe/vxmarw.jpg';
  const testPrompt = 'ubah gayanya';
  const endpoint = settings.editEndpoint || DEFAULT_EDIT_ENDPOINT;
  const paramName = settings.editParamName || DEFAULT_EDIT_PARAM;
  const separator = endpoint.includes('?') ? '&' : '?';
  const apiUrl = `${endpoint}${separator}${paramName}=${encodeURIComponent(testImgUrl)}&prompt=${encodeURIComponent(testPrompt)}`;

  addMessage('**Test API Edit Foto**\n\nEndpoint:\n`' + apiUrl + '`\n\nMohon tunggu...', 'bot');

  try {
    const ctrl = new AbortController();
    const timer = setTimeout(() => ctrl.abort(), 30000);
    const res = await fetch(apiUrl, { method: 'GET', signal: ctrl.signal });
    clearTimeout(timer);
    const text = await res.text();
    addMessage(
      '**Hasil:**\n\n' +
      '• Status: ' + (res.ok ? '✅ OK' : '❌ FAIL') + ' (HTTP ' + res.status + ')\n' +
      '• Response preview:\n```\n' + text.slice(0, 300) + '\n```',
      'bot'
    );
  } catch (e) {
    addMessage('**Hasil:** ❌ ' + e.message, 'error');
  }
}

function testVoice() {
  speechUnlocked = false;
  unlockSpeech();
  const voices = window.speechSynthesis.getVoices();
  const langPrefix = settings.lang.split('-')[0];
  const match = voices.find(v => v.lang.startsWith(langPrefix));
  addMessage(
    '**Test Suara**\n\n' +
    'Total voices: ' + voices.length + '\n' +
    'Voice aktif: ' + (match ? match.name + ' (' + match.lang + ')' : 'tidak ada') + '\n' +
    'Bahasa: ' + settings.lang + '\n' +
    'Preset: ' + settings.voicePreset + '\n' +
    'Volume: ' + (settings.volume * 10) + '%',
    'bot'
  );
  speak('Hai! Tes suara. Satu, dua, tiga. Suara AI sudah siap.');
}

/* ============ SEND ============ */
async function sendMessage(customText, fromVoice = false) {
  const text = (customText !== undefined ? customText : input.value).trim();
  if (!text || isBusy) return;

  isBusy = true;
  sendBtn.disabled = true;
  input.value = '';
  input.style.height = '22px';
  menu.classList.remove('show');

  const att = attachedImage;
  addMessage(text, 'user', att ? att.dataUrl : null);
  attachedImage = null;
  attachBar.classList.remove('show');

  const wantsEdit  = isEditRequest(text);
  const wantsVideo = isVideoRequest(text);

  logDebug('Deteksi', { text, wantsEdit, wantsVideo, hasAttach: !!att });

  try {
    if (att && wantsEdit) {
      createEditProgress();
      setEditStep('upload', 'active');

      let publicUrl;
      try {
        publicUrl = await uploadPhoto(att.dataUrl);
        setEditStep('upload', 'done');
        logDebug('Upload selesai', publicUrl);
      } catch (upErr) {
        finishEditProgress();
        logDebug('Upload gagal', upErr.message);
        addMessage('❌ Upload gagal: ' + upErr.message, 'error');
        if (settings.speak) speak('Upload gagal.', () => { if (fromVoice && loopMode && inVoiceOverlay) resumeVoiceLoop(); });
        return;
      }

      setEditStep('request', 'active');
      let resultUrl;
      try {
        resultUrl = await editPhotoApi(publicUrl, text);
        setEditStep('request', 'done');
        logDebug('Edit selesai', resultUrl);
      } catch (editErr) {
        finishEditProgress();
        logDebug('Edit gagal', editErr.message);
        addMessage('❌ Edit gagal: ' + editErr.message, 'error');
        if (settings.speak) speak('Edit gagal.', () => { if (fromVoice && loopMode && inVoiceOverlay) resumeVoiceLoop(); });
        return;
      }

      setEditStep('render', 'active');
      finishEditProgress();
      addImageMessage(resultUrl, text);

      if (settings.speak) speak('Foto selesai diproses.', () => {
        if (fromVoice && loopMode && inVoiceOverlay) resumeVoiceLoop();
      });
      return;
    }

    if (wantsEdit && !att) {
      addMessage('Untuk **edit foto asli**, silakan **lampirkan foto dulu** ya (klik ikon gambar di kiri bawah), lalu ketik perintah edit.', 'bot');
      if (settings.speak) speak('Lampirkan foto dulu ya.', () => { if (fromVoice && loopMode && inVoiceOverlay) resumeVoiceLoop(); });
      return;
    }

    if (wantsVideo) {
      addTyping();
      const { url, prompt } = await generateVideo(text);
      rmTyping();
      addMessage('Video berhasil dibuat.', 'bot');
      addVideoMessage(url, prompt);
      if (settings.speak) speak('Video sedang dibuat.', () => { if (fromVoice && loopMode && inVoiceOverlay) resumeVoiceLoop(); });
      return;
    }

    addTyping();
    const sysPrompt = 'Kamu GrenX, asisten AI santai, hangat, ngobrol kayak teman. Bahasa sehari-hari, jangan kaku, jangan formal. Sesuaikan bahasa dengan user.\n\nUser: ' + text;
    const reply = await callChat(sysPrompt);
    rmTyping();
    addMessage(reply, 'bot');
    if (settings.speak) {
      speak(reply, () => { if (fromVoice && loopMode && inVoiceOverlay) resumeVoiceLoop(); });
    } else {
      if (fromVoice && loopMode && inVoiceOverlay) resumeVoiceLoop();
    }

  } catch (err) {
    rmTyping();
    let msg = err.message || 'Gagal';
    if (msg.includes('Failed to fetch') || msg.includes('NetworkError') || msg.includes('Load failed')) {
      msg = 'Koneksi gagal. Coba lagi.';
    }
    addMessage('Gagal: ' + msg, 'error');
    if (settings.speak) speak('Maaf, terjadi kesalahan.', () => { if (fromVoice && loopMode && inVoiceOverlay) resumeVoiceLoop(); });
  } finally {
    isBusy = false;
    sendBtn.disabled = false;
    if (!fromVoice) input.focus();
  }
}

sendBtn.addEventListener('click', () => sendMessage());
input.addEventListener('keydown', e => {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); sendMessage(); }
});
input.addEventListener('input', () => {
  input.style.height = '22px';
  const h = Math.min(input.scrollHeight, 90);
  input.style.height = h + 'px';
});
input.addEventListener('focus', () => inputBar.classList.add('focus'));
input.addEventListener('blur', () => inputBar.classList.remove('focus'));

function quickAsk(q){ input.value = q; sendMessage(q); }

/* ============ VOICE RECOGNITION ============ */
function initRecognition() {
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  if (!SR) return null;
  const rec = new SR();
  rec.continuous = false;
  rec.interimResults = true;
  rec.lang = settings.lang;
  rec.maxAlternatives = 1;
  rec.onstart = () => {
    listening = true;
    micBtn.classList.add('mic-active');
    voiceStatus.textContent = 'Mendengarkan...';
    voiceHint.textContent = 'Bicara sekarang, AI akan jawab';
    voiceTranscript.textContent = '';
    lastTranscript = '';
  };
  rec.onresult = (e) => {
    let transcript = '';
    for (let i = 0; i < e.results.length; i++) transcript += e.results[i][0].transcript;
    transcript = transcript.trim();
    lastTranscript = transcript;
    voiceTranscript.textContent = '"' + transcript + '"';
    clearTimeout(finalizeTimer);
    finalizeTimer = setTimeout(() => {
      const text = lastTranscript.trim();
      if (!text) return;
      voiceStatus.textContent = 'Dikirim ke AI...';
      voiceHint.textContent = 'AI sedang berpikir';
      stopRecognitionOnly();
      sendMessage(text, true);
    }, 1200);
  };
  rec.onerror = (e) => {
    if (e.error === 'not-allowed') { showToast('Izin mikrofon ditolak', true); stopVoice(); return; }
    if (e.error === 'no-speech' || e.error === 'aborted') return;
    if (inVoiceOverlay && loopMode) setTimeout(() => { if (inVoiceOverlay) startVoice(); }, 500);
  };
  rec.onend = () => {
    listening = false;
    micBtn.classList.remove('mic-active');
    if (inVoiceOverlay && loopMode && !isBusy && !lastTranscript.trim()) {
      setTimeout(() => { if (inVoiceOverlay && !listening) startVoice(); }, 400);
    }
  };
  return rec;
}
function startVoice() {
  if (!recognition) recognition = initRecognition();
  if (!recognition) { showToast('Browser tidak support voice. Pakai Chrome.', true); return; }
  try {
    recognition.lang = settings.lang;
    listening = true;
    recognition.start();
  } catch (e) {
    if (e.message && e.message.includes('already started')) return;
    recognition = initRecognition();
    if (recognition) {
      try {
        recognition.lang = settings.lang;
        listening = true;
        recognition.start();
      } catch (e2) { showToast('Gagal start voice: ' + e2.message, true); listening = false; }
    }
  }
}
function stopRecognitionOnly() {
  if (recognition) { try { recognition.stop(); } catch {} }
  listening = false;
  micBtn.classList.remove('mic-active');
}
function stopVoice() {
  inVoiceOverlay = false;
  listening = false;
  clearTimeout(finalizeTimer);
  stopRecognitionOnly();
  stopSpeak();
  voiceOverlay.classList.remove('show');
}
function resumeVoiceLoop() {
  if (!inVoiceOverlay || !loopMode) return;
  setTimeout(() => {
    if (!inVoiceOverlay || isBusy) return;
    startVoice();
  }, 400);
}

micBtn.addEventListener('click', async () => {
  if (listening) { stopVoice(); return; }
  if (!micPermissionGranted) { const ok = await requestMicPermission(); if (!ok) return; }
  inVoiceOverlay = true;
  voiceOverlay.classList.add('show');
  startVoice();
});
voiceModeBtn.addEventListener('click', async () => {
  if (!micPermissionGranted) { const ok = await requestMicPermission(); if (!ok) return; }
  inVoiceOverlay = true;
  voiceOverlay.classList.add('show');
  startVoice();
});
voiceClose.addEventListener('click', stopVoice);

/* ============ MENU ============ */
menuBtn.addEventListener('click', (e) => {
  e.stopPropagation();
  menu.classList.toggle('show');
});
document.addEventListener('click', (e) => {
  if (!menu.contains(e.target) && !menuBtn.contains(e.target)) menu.classList.remove('show');
});

function menuAction(t) {
  menu.classList.remove('show');
  switch (t) {
    case 'voice':
      (async () => {
        if (!micPermissionGranted) { const ok = await requestMicPermission(); if (!ok) return; }
        inVoiceOverlay = true;
        voiceOverlay.classList.add('show');
        startVoice();
      })();
      break;
    case 'camera': openCamera(); break;
    case 'video': quickAsk('buat video anime girl dancing, neon city'); break;
    case 'editFoto':
      showToast('Lampirkan foto dulu, lalu ketik perintah edit');
      fileInput.click();
      break;
    case 'voiceSelect':
      settingsPanel.classList.add('show');
      loadSettingsToUI();
      showToast('Pilih suara AI di bawah');
      break;
    case 'testVoice': testVoice(); break;
    case 'testApi': testAllApis(); break;
    case 'testEdit': testEditApi(); break;
    case 'clear':
      stopSpeak();
      chat.innerHTML = `
        <div class="welcome" id="welcome">
          <div class="welcome-avatar">G</div>
          <h2>GrenX Voice Studio</h2>
          <p>Ngobrol santai, pakai suara, foto live, generate video, edit foto asli. Semua dalam satu.</p>
          <div class="welcome-chips">
            <div class="chip" onclick="quickAsk('hai, kenalan dong')">Ngobrol</div>
            <div class="chip" onclick="menuAction('voice')">Voice Mode</div>
            <div class="chip" onclick="menuAction('camera')">Foto Live</div>
            <div class="chip" onclick="menuAction('editFoto')">Edit Foto</div>
          </div>
        </div>`;
      showToast('Chat dibersihkan');
      break;
  }
}

/* ============ ENDPOINT PRESETS ============ */
function setEditEndpointPreset(url) {
  document.getElementById('setEditEndpoint').value = url;
  if (url.includes('nanobanana')) {
    document.getElementById('setEditParamName').value = 'image';
  } else {
    document.getElementById('setEditParamName').value = 'url';
  }
  updateEndpointPreview();
}
function updateEndpointPreview() {
  const ep = document.getElementById('setEditEndpoint').value || DEFAULT_EDIT_ENDPOINT;
  const param = document.getElementById('setEditParamName').value || 'url';
  const sep = ep.includes('?') ? '&' : '?';
  const preview = `${ep}${sep}${param}=[URL_FOTO]&prompt=[PERINTAH]`;
  document.getElementById('currentEndpointPreview').textContent = preview;
}

/* ============ SETTINGS ============ */
function loadSettingsToUI() {
  document.getElementById('setEditEndpoint').value = settings.editEndpoint || DEFAULT_EDIT_ENDPOINT;
  document.getElementById('setEditParamName').value = settings.editParamName || DEFAULT_EDIT_PARAM;
  document.getElementById('setEndpoint').value = settings.endpoint;
  document.getElementById('setTimeout').value = settings.timeout;
  document.getElementById('setTimeoutVal').textContent = settings.timeout + 's';
  document.getElementById('setFontSize').value = settings.fontSize;
  document.getElementById('setFontSizeVal').textContent = settings.fontSize + 'px';
  document.getElementById('setSpeak').checked = settings.speak;
  document.getElementById('setLoop').checked = settings.loop;
  document.getElementById('setVoicePreset').value = settings.voicePreset;
  document.getElementById('setLang').value = settings.lang;
  document.getElementById('setRate').value = settings.rate;
  document.getElementById('setRateVal').textContent = (settings.rate/10).toFixed(1) + 'x';
  document.getElementById('setPitch').value = settings.pitch;
  document.getElementById('setPitchVal').textContent = (settings.pitch/10).toFixed(1);
  document.getElementById('setVolume').value = settings.volume;
  document.getElementById('setVolumeVal').textContent = (settings.volume*10) + '%';
  document.getElementById('setVidQuality').value = settings.vidQuality;
  document.getElementById('setCameraFacing').value = settings.cameraFacing;
  document.getElementById('setAutoSave').checked = settings.autoSave;
  document.getElementById('setTheme').value = settings.theme;
  document.getElementById('setCompact').checked = settings.compact;
  updateEndpointPreview();
}
settingsBtn.addEventListener('click', () => { loadSettingsToUI(); settingsPanel.classList.add('show'); });
settingsClose.addEventListener('click', () => settingsPanel.classList.remove('show'));
settingsPanel.addEventListener('click', (e) => { if (e.target === settingsPanel) settingsPanel.classList.remove('show'); });

document.getElementById('setEditEndpoint').addEventListener('input', updateEndpointPreview);
document.getElementById('setEditParamName').addEventListener('change', updateEndpointPreview);
document.getElementById('setTimeout').addEventListener('input', e => { document.getElementById('setTimeoutVal').textContent = e.target.value + 's'; });
document.getElementById('setRate').addEventListener('input', e => { document.getElementById('setRateVal').textContent = (e.target.value/10).toFixed(1) + 'x'; });
document.getElementById('setPitch').addEventListener('input', e => { document.getElementById('setPitchVal').textContent = (e.target.value/10).toFixed(1); });
document.getElementById('setVolume').addEventListener('input', e => { document.getElementById('setVolumeVal').textContent = (e.target.value*10) + '%'; });
document.getElementById('setFontSize').addEventListener('input', e => { document.getElementById('setFontSizeVal').textContent = e.target.value + 'px'; });

document.getElementById('saveSettings').addEventListener('click', () => {
  settings.editEndpoint = document.getElementById('setEditEndpoint').value.trim() || DEFAULT_EDIT_ENDPOINT;
  settings.editParamName = document.getElementById('setEditParamName').value || DEFAULT_EDIT_PARAM;
  settings.endpoint = document.getElementById('setEndpoint').value;
  settings.timeout = parseInt(document.getElementById('setTimeout').value) || 30;
  settings.fontSize = parseInt(document.getElementById('setFontSize').value) || 15;
  settings.speak = document.getElementById('setSpeak').checked;
  settings.loop = document.getElementById('setLoop').checked;
  loopMode = settings.loop;
  settings.voicePreset = document.getElementById('setVoicePreset').value;
  settings.lang = document.getElementById('setLang').value;
  settings.rate = parseInt(document.getElementById('setRate').value) || 10;
  settings.pitch = parseInt(document.getElementById('setPitch').value) || 10;
  settings.volume = parseInt(document.getElementById('setVolume').value) || 10;
  settings.vidQuality = document.getElementById('setVidQuality').value;
  settings.cameraFacing = document.getElementById('setCameraFacing').value;
  settings.autoSave = document.getElementById('setAutoSave').checked;
  settings.theme = document.getElementById('setTheme').value;
  settings.compact = document.getElementById('setCompact').checked;
  persist();
  applyTheme();
  applyFontSize();
  applyCompact();
  settingsPanel.classList.remove('show');
  showToast('Pengaturan disimpan');
  if (settings.speak) {
    const names = { default: 'suara default', friendly: 'suara ramah', cheerful: 'suara ceria', calm: 'suara tenang', serious: 'suara tegas' };
    speak('Hai! Sekarang pakai ' + (names[settings.voicePreset] || 'suara baru') + '.');
  }
});

/* ============ EXPORT / IMPORT / RESET ============ */
document.getElementById('exportSettings').addEventListener('click', () => {
  const blob = new Blob([JSON.stringify(settings, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'grenx-settings-' + Date.now() + '.json';
  a.click();
  URL.revokeObjectURL(url);
  showToast('Pengaturan diexport');
});
document.getElementById('importSettings').addEventListener('click', () => { document.getElementById('importFile').click(); });
document.getElementById('importFile').addEventListener('change', (e) => {
  const f = e.target.files?.[0];
  if (!f) return;
  const reader = new FileReader();
  reader.onload = (ev) => {
    try {
      const data = JSON.parse(ev.target.result);
      Object.assign(settings, DEFAULTS, data);
      loopMode = settings.loop;
      persist();
      loadSettingsToUI();
      applyTheme(); applyFontSize(); applyCompact();
      showToast('Pengaturan diimport');
    } catch (err) { showToast('File tidak valid', true); }
  };
  reader.readAsText(f);
  e.target.value = '';
});
document.getElementById('resetSettings').addEventListener('click', () => {
  if (!confirm('Reset semua pengaturan ke default?')) return;
  settings = { ...DEFAULTS };
  loopMode = settings.loop;
  persist();
  loadSettingsToUI();
  applyTheme(); applyFontSize(); applyCompact();
  showToast('Pengaturan direset');
});

/* ============ INIT VOICES ============ */
if ('speechSynthesis' in window) {
  window.speechSynthesis.getVoices();
  window.speechSynthesis.onvoiceschanged = () => window.speechSynthesis.getVoices();
  let retry = 0;
  const iv = setInterval(() => {
    const v = window.speechSynthesis.getVoices();
    if (v.length > 0 || retry > 15) clearInterval(iv);
    retry++;
  }, 500);
}

logDebug('App loaded', {
  protocol: location.protocol,
  isSecureContext: window.isSecureContext,
  editEndpoint: settings.editEndpoint,
  editParamName: settings.editParamName
});

input.focus();
</script>
</body>
</html>
