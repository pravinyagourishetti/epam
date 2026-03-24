renderedCallback() {
    if (this.isVerifiedQuestionsAvailable) {

        const userInputs = this.template.querySelectorAll('[data-type="user-input"]');

        if (userInputs && userInputs.length > 0) {
            // Focus first question only
            userInputs[0].focus();
        }

        this.isVerifiedQuestionsAvailable = false;
    }
}

<template if:true={modalisopen}>

<c-care_confirmation-dialog
    title={Care_CloseReason.Pop_Up_Header}
    message={Care_CloseReason.Pop_Up_Message}
    cancel-label={Care_CloseReason.Cancel_Label}
    confirm-label={Care_CloseReason.ConfirmLabel}
    visible={showCloseIdnvConfirmation}
    name="confirmmodal"
    onclosemodalclick={handleConfirmDialog}>
</c-care_confirmation-dialog>

<template if:false={isStepUpApi}>

<section role="dialog"
         tabindex="-1"
         aria-labelledby="modal-heading-01"
         aria-modal="true"
         class="slds-modal slds-fade-in-open">

<div data-id="modal_container" class="slds-modal__container">

<div class="slds-modal__header slds-card mainheader">

<h1 id="modal-heading-01"
    class="slds-text-heading_medium slds-hyphenate">
    Locate Customer
</h1>

</div>

<div class="slds-modal__content spinnerPosition"
     id="modal-content-id-1">

<template if:true={submitspinner}>
    <lightning-spinner alternative-text="Loading" size="medium" variant="brand"></lightning-spinner>
</template>

<template if:true={basicIDnVLoading}>
    <div style="height:100px;" class="slds-box slds-box_small">
        <lightning-spinner alternative-text="Loading" size="medium" variant="brand"></lightning-spinner>
    </div>
</template>

<div class="slds-p-around_small">

<template if:true={agentLandingButton}>
    <p tabindex="0">{agentLandingDisplayText}</p>
</template>

<template if:false={agentLandingButton}>

<template if:true={resultData.questions}>

<!-- Intent -->
<template if:true={intentvisibility}>
    <div class="containterInt">
        <label>Call Intent: </label>
        <label>{callintentvalue}</label>
    </div>
    <br />
</template>

<!-- QUESTIONS LOOP -->
<template for:each={resultData.questions} for:item="q" for:index="index">

<div key={q.id}>

<div style="display: flex;">

<label class="slds-form-element_label bold-label"
       id={q.id}>
       {q.text}
       <span style="color: red;">*</span>
</label>

</div>

<lightning-input
    type={q.type}
    date-style="short"
    name={q.id}
    required
    autocomplete="off"
    variant="label-hidden"
    maxlength={q.maxlength}
    onchange={handleValidations}
    data-type="user-input"
    aria-labelledby={q.id}>
</lightning-input>

<template for:each={q.autoFillCheckboxList} for:item="eachCheckBox">

<div key={eachCheckBox.dob} class={eachCheckBox.className}>

<lightning-input
    class={eachCheckBox.inputClassName}
    type="checkbox"
    onclick={handleCheckBoxOnChange}>
</lightning-input>

<label class="slds-form-element_label bold-label">
    {eachCheckBox.label}
</label>

</div>

</template>

</div>

</template>

</template>

<!-- DATATABLE -->
<template if:true={isCustomersFound}>

<div class="slds-scrollable_x slds-scrollable_y scroller-custom slds-box slds-box_xx-small"
     id="modal-datatable-01"
     style="height: 300px;">

<lightning-datatable
    key-field="id"
    data={accountsList}
    columns={columns}
    hide-checkbox-column
    resize-column-disabled
    column-widths-mode="auto"
    min-column-width="150"
    onrowaction={openCustomerLanding}
    aria-labelledby="modal-datatable-01">
</lightning-datatable>

</div>

</template>

<!-- Approve API -->
<template if:true={isApproveApi}>
<div class="slds-box slds-box_small">
<p aria-live="polite">
    <lightning-formatted-text value="Approve API is called">
    </lightning-formatted-text>
</p>
</div>
</template>

</div>
</div>

<!-- FOOTER -->
<div class="slds-modal__footer">

<template if:false={agentLandingButton}>
<template if:true={resultData.questions}>

<template if:true={isDisplayUnidentifiedatn}>

<lightning-button
    label={label.Unidentified_Prospect_Button}
    title={label.Unidentified_Prospect_Button}
    onclick={openworkflowlav}
    icon-name="utility:user"
    variant="neutral"
    class="toleftalign slds-m-left_x-small">
</lightning-button>

</template>

<div class="idnvFooter">

<lightning-combobox
    label="Call Dropped Reason"
    value={value}
    variant="label-hidden"
    placeholder={Care_CloseReason.Pop_Up_Header}
    options={options}
    onchange={handleCallDropdownChange}
    class="callDropReasonCombobox">
</lightning-combobox>

<template if:true={showBypassBtn}>
<lightning-button
    variant="brand"
    label="ByPass"
    onclick={handleBypassButton}
    disabled={bypassButton.disabled}>
</lightning-button>
</template>

<lightning-button
    variant="brand"
    label="Verify"
    aria-label="Verify"
    onclick={handlesubmit}
    disabled={checkvalidity}>
</lightning-button>

</div>

</template>
</template>

<template if:true={agentLandingButton}>
<lightning-button
    variant="brand"
    label={btnAgentLandingText}
    onclick={handleReloadButtuon}>
</lightning-button>
</template>

</div>

</div>

<!-- Workflow -->
<div data-id="workflowPopup">

<template if:true={showworkflow}>

<lightning-icon
    alternative-text="Close"
    icon-name="utility:close"
    onclick={closeworkflowNav}
    size="x-medium">
</lightning-icon>

<c-care_unidentifyworkflow
    account-info={accountInfo}
    unidentifyinfo={unidentifyinfo}
    onclose={closemodalworkflow}
    ucontactid={ucontactid}
    idnvobj={idnvobj}>
</c-care_unidentifyworkflow>

</template>

</div>

</section>

<div class="slds-backdrop slds-backdrop_open"></div>

</template>
</template>
