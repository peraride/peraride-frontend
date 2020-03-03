<template>
	<v-container>
			<h3 style="text-align:center;">Dock Management</h3>
            <br>
            <DockForm />
	</v-container>
</template>

<script>
import DockForm from '../../components/DockForm'

	export default {
          components: {
             // Navbar,
            DockForm
        },
		middleware: 'auth-admin'
	}
</script>