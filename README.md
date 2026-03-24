<template>

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
         aria-describedby="modal-content-id-1"
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

<!-- QUESTIONS -->
<template for:each={resultData.questions} for:item="q">

<div key={q.id}>

<label class="slds-form-element_label bold-label"
       id={q.id}>
       {q.text}
       <span style="color: red;">*</span>
</label>

<lightning-input
    type={q.type}
    name={q.id}
    required
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

<!-- APPROVE -->
<template if:true={isApproveApi}>
<div class="slds-box slds-box_small">
<p aria-live="polite">
    Approve API is called
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

</section>

<div class="slds-backdrop slds-backdrop_open"></div>

</template>
</template>
