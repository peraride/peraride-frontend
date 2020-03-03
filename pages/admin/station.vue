<template>
	<v-container>
			<h3>Station Management</h3>
            <br>
            <StationForm />
	</v-container>
</template>

<script>
import StationForm from '../../components/StationForm'

	export default {
          components: {
             // Navbar,
            StationForm
        },
		middleware: 'auth-admin'
	}
</script>