// ADDED: Step 3 Enter handler – compute net and go next
_onStep3Enter: function () {
    var oGW = this.byId("ipGrossWeight");
    var oTW = this.byId("ipTareWeight");
    var oNW = this.byId("ipNetWeight");

    var gw = oGW ? parseFloat(oGW.getValue()) : NaN;
    var tw = oTW ? parseFloat(oTW.getValue()) : NaN;

    if (isNaN(gw) || isNaN(tw)) {
        sap.m.MessageToast.show("Enter valid numbers for Gross and Tare.");
        return;
    }
    if (oNW) { oNW.setValue(gw - tw); }

    // mark valid & advance
    this.oWizard.validateStep(this.byId("step3"));
    this.oWizard.nextStep();
},

// ADDED: one-time wiring of Enter → nextStep
_setupEnterToNext: function () {
    if (this._enterWired) { return; } // avoid duplicate delegates on re-render
    this._enterWired = true;

    // STEP 1: Enter on contract input → run your existing onNextStep (does action + next)
    var oIp = this.byId("ipContract");
    if (oIp) {
        oIp.addEventDelegate({
            onsapenter: function () { this.onNextStep(); }.bind(this)
        });
    }

    // STEP 3: Enter on weight fields → compute & advance
    var oGW = this.byId("ipGrossWeight");
    var oTW = this.byId("ipTareWeight");

    [oGW, oTW].forEach(function (oCtrl) {
        if (oCtrl) {
            oCtrl.addEventDelegate({
                onsapenter: function () { this._onStep3Enter(); }.bind(this)
            });
        }
    }.bind(this));
}

onAfterRendering: function(){
    var oIp = this.byId("ipContract");
    if (oIp) { oIp.focus(); }

    // ADDED: hook Enter → next only once
    this._setupEnterToNext();
},
